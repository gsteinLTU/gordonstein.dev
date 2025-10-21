<script lang="ts">
    type Suit = 'Hearts' | 'Diamonds' | 'Clubs' | 'Spades';
    type Rank = '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9' | '10' | 'J' | 'Q' | 'K' | 'A';

    export interface PlayingCardProps {
        suit: Suit;
        rank: Rank;
        faceUp?: boolean;
    }
    

    const { suit, rank, faceUp = true }: PlayingCardProps = $props();

    const suitSymbols: Record<Suit, string> = {
        'Hearts': '♥',
        'Diamonds': '♦',
        'Clubs': '♣',
        'Spades': '♠'
    };

    const color = (suit === 'Hearts' || suit === 'Diamonds') ? 'red' : 'black';

</script>

{#if faceUp}
    <div class="card" style="color: {color};">
        <div class="corner top-left">
            {rank}{suitSymbols[suit]}
        </div>  
        <div class="center">
            {#if rank === 'J'}
                J
            {:else if rank === 'Q'}
                Q
            {:else if rank === 'K'}
                K
            {:else if rank === 'A'}            
                A
            {:else}
                {rank}
            {/if}
        </div>
        <div class="corner bottom-right">
            {rank}{suitSymbols[suit]}
        </div>
    </div>
{:else}
    <div class="card back">
        <!-- Back of the card -->
    </div>
{/if}

<style>
    .card {
        width: 100px;
        height: 150px;
        border: 1px solid #000;
        border-radius: 8px;
        display: block;
        position: relative;
        justify-content: center;
        align-items: center;
        font-size: 24px;
        background-color: white;
        user-select: none;
    }

    .card.back {
        background-color: white;
        border: #000 1px solid;
    }

    .card.back::after {
        content: "";
        position: absolute;
        top: 0.2em;
        left: 0.2em;
        right: 0.2em;
        bottom: 0.2em;
        border-radius: 8px;
        background: repeating-linear-gradient(-45deg,  transparent 0, transparent 48%, rgba(255, 255, 255, 1) 49%,rgba(255, 255, 255, 1) 51%, transparent 52%, transparent 100%), repeating-linear-gradient(45deg, transparent 0%, transparent 48%, rgba(255, 255, 255, 1) 49%,rgba(255, 255, 255, 1) 51%, transparent 52%,  transparent 100%), linear-gradient(rgb(30, 30, 134), rgb(30, 30, 134));
        background-size: 20px 20px, 20px 20px, auto;
    }

    .corner {
        position: absolute;
        font-size: 16px;
    }

    .center {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        font-size: 48px;
    }

    .top-left {
        top: 8px;
        left: 8px;
    }

    .bottom-right {
        bottom: 8px;
        right: 8px;
        transform: rotate(180deg);
    }
</style>