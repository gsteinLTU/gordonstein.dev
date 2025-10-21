<script lang="ts">
    import { browser } from "$app/environment";
    import PlayingCard, { type PlayingCardProps } from "$lib/components/demos/blackjack-rl/PlayingCard.svelte";
    import { onDestroy } from "svelte";

    let playerHand: PlayingCardProps[] = $state([]);

    let dealerHand: PlayingCardProps[] = $state([]);

    let gameState: 'player-turn' | 'dealer-turn' | 'game-over' = $state('player-turn');
    let deck: PlayingCardProps[] = [];
    let log: string[] = $state([]);
    let logDiv: HTMLDivElement | null = null;
    let playerWins = $state(0);
    let resultsLast100: number[] = $state([]);
    let gamesPlayed = $state(0);

    $effect(() => {
        if(log && logDiv){
            logDiv.scrollTop = logDiv.scrollHeight;
        }
    });

    /**
     * Randomize the deck
     */
    function shuffleDeck() {
        for (let i = deck.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [deck[i], deck[j]] = [deck[j], deck[i]];
        }
    }

    /**
     * Add a deck to the shoe
     */
    function addDeck() {
        const suits: PlayingCardProps['suit'][] = ['Hearts', 'Diamonds', 'Clubs', 'Spades'];
        const ranks: PlayingCardProps['rank'][] = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];
        deck = [];
        for (const suit of suits) {
            for (const rank of ranks) {
                deck.push({ suit, rank, faceUp: false });
            }
        }
        shuffleDeck();
    }

    const decks = 1;

    for (let i = 0; i < decks; i++) {
        addDeck();
    }

    /**
     * Deal the top card on the deck into one of the hands
     * @param to Hand to deal to
     * @param faceUp If the card should be added as a face-up card
     */
    function dealCard(to: 'player' | 'dealer', faceUp: boolean = true, addDeckIfEmpty = true) {
        if(deck.length == 0 && addDeckIfEmpty){
            addDeck();
            shuffleDeck();
        }
        
        const card = deck.pop();
        if (card) {
            card.faceUp = faceUp;
            if (to === 'player') {
                playerHand = [...playerHand, card];
            } else {
                dealerHand = [...dealerHand, card];
            }
        } else {
            console.error("The deck is empty!");
        }
    }


    function initialDeal() {
        playerHand = [];
        dealerHand = [];

        dealCard('player', true);
        dealCard('dealer', false);
        dealCard('player', true);
        dealCard('dealer', true);
    }

    // Tabular Q-Learning Agent

    type Action = 'hit' | 'stand';
    type State = [playerTotal: number, dealerUpCard: number];

    function stateKey(state: State): string {
        return `${state[0]}:${state[1]}`;
    }

    let qTableVersion = $state(0);
    let playerBrain = {
        qTable: new Map<string, Map<Action, number>>(),
        epsilon: 0.1,
        alpha: 0.5,
        gamma: 0.9
    };

    /**
     * Get value of a card
     * @param card Numeric value of card, Aces will be given 11
     */
    function getCardValue(card: PlayingCardProps): number {
        if (card.rank === 'A') return 11;
        if (['K', 'Q', 'J'].includes(card.rank)) return 10;
        return parseInt(card.rank);
    }

    /**
     * Calculate the total value of a blackjack hand.
     * @param hand List of cards
     */
    function calculateHandValue(hand: PlayingCardProps[]): number {
        let value = 0;
        let aces = 0;

        for (const card of hand) {
            value += getCardValue(card);
            if (card.rank === 'A') aces += 1;
        }

        // Prevent busting with Aces if possible
        while (value > 21 && aces > 0) {
            value -= 10;
            aces -= 1;
        }

        return value;
    }

    function getState(): State {
        const playerTotal = calculateHandValue(playerHand);
        const dealerUpCard = getCardValue(dealerHand[1]);
        return [playerTotal, dealerUpCard];
    }

    function chooseAction(state: State): Action {
        if (Math.random() < playerBrain.epsilon) {
            return Math.random() < 0.5 ? 'hit' : 'stand';
        } else {
            const actions = playerBrain.qTable.get(stateKey(state));
            if (actions) {
                return actions.get('hit')! >= actions.get('stand')! ? 'hit' : 'stand';
            } else {
                return Math.random() < 0.5 ? 'hit' : 'stand';
            }
        }
    }

    function updateQTable(state: State, action: Action, reward: number) {
        const key = stateKey(state);
        let actions = playerBrain.qTable.get(key);
        if (!actions) {
            actions = new Map<Action, number>([['hit', 0], ['stand', 0]]);
            playerBrain.qTable.set(key, actions);
        }

        const currentQ = actions.get(action)!;
        const maxNextQ = Math.max(...Array.from(actions.values()));
        const newQ = currentQ + playerBrain.alpha * (reward + playerBrain.gamma * maxNextQ - currentQ);
        actions.set(action, newQ);
        qTableVersion++;
    }

    /**
     * Play a round of blackjack
     */
    function playRound() {
        console.log('Starting round');
        initialDeal();
        let reward = 0;
        gameState = 'player-turn';
        while (gameState === 'player-turn') {
            const state = getState();
            console.log(`State is ${state}`);
            const action = chooseAction(state);
            if (action === 'hit') {
                dealCard('player', true);
                const playerTotal = calculateHandValue(playerHand);
                if (playerTotal > 21) {
                    reward = -1;
                    updateQTable(state, action, reward); // Player busts
                    log = [...log, 'Player Busts!'];
                    gameState = 'game-over';
                    
                } else {
                    updateQTable(state, action, reward); // No immediate reward
                }
            } else {
                updateQTable(state, action, reward); // No immediate reward
                gameState = 'dealer-turn';
            }
        }

        if (gameState === 'dealer-turn') {
            while (calculateHandValue(dealerHand) < 17) {
                dealCard('dealer', true);
            }

            // Reveal dealer's hidden card
            dealerHand[0].faceUp = true;

            // Determine outcome
            const playerTotal = calculateHandValue(playerHand);
            const dealerTotal = calculateHandValue(dealerHand);
            if (dealerTotal > 21 || playerTotal > dealerTotal) {
                if (dealerTotal > 21){
                    log = [...log, 'Dealer Busts!'];
                }
                log = [...log, `Player wins (${playerTotal} vs ${dealerTotal})!`];
                reward = 1; // Player wins
                playerWins++;
            } else if (playerTotal < dealerTotal) {
                log = [...log, `Dealer wins (${dealerTotal} vs ${playerTotal})!`];
                reward = -1; // Dealer wins
            }
            const state = getState();
            updateQTable(state, 'stand', reward);
            gameState = 'game-over';
        }
    gamesPlayed++;
    resultsLast100 = [...resultsLast100, reward].slice(-100);
    console.log(playerBrain);
    }

    /**
     * Get the most likely strategy for the State, or null if none is known yet
     * @param state State to look up in Q table
     */
    function getStrategy(state: State): Action | null {
        const actions = playerBrain.qTable.get(stateKey(state));
        if (actions) {
            return actions.get('hit')! >= actions.get('stand')! ? 'hit' : 'stand';
        }
        return null;
    }
    
    if (browser) {
        let playLoopTimeout: NodeJS.Timeout;
        let playLoop = () => {
            playRound();
            playLoopTimeout = setTimeout(playLoop, 1);
        };
        playLoop();
        onDestroy(() => {
            if(playLoopTimeout) {
                clearTimeout(playLoopTimeout);
            }
        })
    }
</script>

<h2>Blackjack RL Demo</h2>
<h3>
    Player Hand ({calculateHandValue(playerHand)}):
</h3>
<div class="hand">
    {#each playerHand as card}
        <PlayingCard suit={card.suit} rank={card.rank} faceUp={card.faceUp} />
    {/each}
</div>

<h3>
    Dealer Hand ({calculateHandValue(dealerHand)}):
</h3>
<div class="hand">
    {#each dealerHand as card}
        <PlayingCard suit={card.suit} rank={card.rank} faceUp={card.faceUp} />
    {/each}
</div>

<h3>
    Strategy Visualizer
</h3>
<div class="strategy-table-wrapper">
    {#key qTableVersion}
        <div class="strategy-table-labels">
            <div class="axis-label axis-label-x">Dealer's Shown Card →</div>
            <div class="axis-label axis-label-y">Player's Hand ↓</div>
            <table class="strategy-table">
                <thead>
                    <tr>
                        <th class="sticky-col"></th>
                        {#each Array(10) as _, di}
                            <th>{(di + 2) != 11 ? (di + 2) : 'A'}</th>
                        {/each}
                    </tr>
                </thead>
                <tbody>
                    {#each Array(17) as _, pi}
                        <tr>
                            <th class="sticky-col">{pi + 4}</th>
                            {#each Array(10) as _, di}
                                <td class="stratcell {getStrategy([pi + 4, di + 2]) ?? 'unknown'}"></td>
                            {/each}
                        </tr>
                    {/each}
                </tbody>
            </table>
        </div>
    {/key}
</div>

<h3>
    Stats:
</h3>
<div>
    <p>Games played: {gamesPlayed}</p>
    <p>Player win percentage (since beginning): <span class="font-mono">{(playerWins / gamesPlayed * 100).toFixed(2)}%</span></p>
    <p>Player win percentage (last 100 rounds): <span class="font-mono">{(resultsLast100.filter(r => r === 1).length / resultsLast100.length * 100).toFixed(2)}%</span></p>
</div>


<h3>
    Log: <button class="clear-btn" onclick={() => log = []}>Clear</button>
</h3>
<div class="log" bind:this={logDiv}>
    {#each log as logItem}
        <p>
        {logItem}
        </p>
    {/each}
</div>


<style>
    h2 {
        margin-bottom: 10px;
        margin-top: 20px;
        font-family: Arial, sans-serif;
        color: #333;
        font-size: 24px;
    }
    .hand {
        display: flex;
        gap: 10px;
        margin-bottom: 20px;
        border: 1px solid #ccc;
        padding: 10px;
        border-radius: 8px;
        background-color: #f9f9f9;
        box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
    }

    .log {
        font-family: 'Courier New', Courier, monospace;
        margin-bottom: 20px;
        border: 1px solid #ccc;
        padding: 10px;
        border-radius: 8px;
        background-color: #f9f9f9;
        box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
        max-height: 30vh;
        overflow: scroll;
    }

    .strategy-table-labels {
        position: relative;
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
    }
    .axis-label {
        font-size: 0.95em;
        color: #666;
        font-family: inherit;
        margin-bottom: 0.2em;
        margin-left: 2.5em;
        font-weight: 500;
        letter-spacing: 0.01em;
    }
    .axis-label-x {
        align-self: center;
        margin-bottom: 0.3em;
        margin-left: 4.5em;
    }
    .axis-label-y {
        position: absolute;
        left: -2.2em;
        top: 4.2em;
        writing-mode: vertical-rl;
        text-orientation: mixed;
        transform: rotate(180deg);
        height: 60%;
        margin: 0;
        z-index: 20;
        background: #fff;
        padding: 0.2em 0.3em 0.2em 0.3em;
        border-radius: 4px;
        font-size: 0.93em;
        box-shadow: 0 1px 4px rgba(0,0,0,0.04);
        border: 1px solid #eee;
        pointer-events: none;

    }
    .strategy-table-labels {
        margin-left: 2.2em;
    }
    .strategy-table-wrapper {
        overflow-x: auto;
        margin-bottom: 2rem;
        border-radius: 10px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.07);
        background: #fff;
        padding: 1rem;
        max-width: 100vw;
        -webkit-overflow-scrolling: touch;
        position: relative;
    }
    .strategy-table {
        border-collapse: collapse;
        width: 100%;
        min-width: 600px;
        font-size: 14px;
        background: #fff;
        table-layout: fixed;
    }
    .strategy-table th, .strategy-table td {
        border: 1px solid #bbb;
        padding: 0.5em 0.7em;
        text-align: center;
        min-width: 2.5em;
        font-family: 'Menlo', 'Consolas', monospace;
        word-break: break-word;
    }
    .strategy-table th {
        background: #f3f3f3;
        font-weight: 600;
        position: sticky;
        top: 0;
        z-index: 2;
    }
    .strategy-table th.sticky-col {
        left: 0;
        position: sticky;
        background: #f3f3f3;
        z-index: 3;
    }
    .strategy-table td.stratcell {
        font-size: 1.1em;
        font-weight: bold;
        transition: background 0.2s;
        cursor: default;
        height: 2.2em;
    }
    .strategy-table td.stratcell.hit {
        background-color: #c6f5c6;
        color: #205c20;
        box-shadow: 0 0 0 1px #a8d3a8 inset;
    }
    .strategy-table td.stratcell.hit::after {
        content: "H";
    }
    .strategy-table td.stratcell.stand {
        background-color: #f5c6c6;
        color: #5c2020;
        box-shadow: 0 0 0 1px #d3a8a8 inset;
    }
    .strategy-table td.stratcell.stand::after {
        content: "S";
    }
    .strategy-table td.stratcell.unknown {
        background-color: #e0e0e0;
        color: #888;
    }
    .strategy-table td.stratcell.unknown::after {
        content: "?";
    }
    .strategy-table tr:hover td.stratcell {
        filter: brightness(0.97);
    }

    .clear-btn {
        margin-left: 0.3em;
        padding: 0.05em 0.5em;
        font-size: 0.8em;
        font-family: inherit;
        background: #f3f3f3;
        border: 1px solid #bbb;
        border-radius: 3px;
        color: #666;
        cursor: pointer;
        transition: background 0.18s, box-shadow 0.18s, color 0.18s;
        box-shadow: none;
        vertical-align: middle;
        line-height: 1.2;
    }
    .clear-btn:hover, .clear-btn:focus {
        background: #e74c3c;
        color: #fff;
        border-color: #e74c3c;
        box-shadow: 0 1px 4px rgba(231,76,60,0.10);
        outline: none;
    }
    .clear-btn:active {
        background: #c0392b;
        color: #fff;
        border-color: #c0392b;
    }

    /* Responsive styles */
    @media (max-width: 900px) {
        .axis-label-y {
            left: -1.2em;
            font-size: 0.85em;
            top: 3.2em;
        }
        .strategy-table-labels {
            margin-left: 1.2em;
        }
        .strategy-table {
            font-size: 12px;
            min-width: 400px;
        }
        .strategy-table th, .strategy-table td {
            padding: 0.3em 0.4em;
            min-width: 1.8em;
        }
    }
    @media (max-width: 600px) {
        .axis-label-y {
            left: -0.5em;
            font-size: 0.75em;
            top: 2.2em;
        }
        .strategy-table-labels {
            margin-left: 0.5em;
        }
        .strategy-table-wrapper {
            padding: 0.3rem;
        }
        .strategy-table {
            font-size: 10px;
            min-width: 300px;
        }
        .strategy-table th, .strategy-table td {
            padding: 0.15em 0.2em;
            min-width: 1.2em;
        }
        .strategy-table th.sticky-col, .strategy-table th {
            font-size: 11px;
        }
    }
</style>