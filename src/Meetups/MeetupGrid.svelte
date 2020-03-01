<script>
    import { createEventDispatcher } from 'svelte';
    import { scale } from 'svelte/transition';
    import { flip } from 'svelte/animate';
    import { cubicIn } from 'svelte/easing';

    import MeetupItem from './MeetupItem.svelte';
    import MeetupFilter from './MeetupFilter.svelte';
    import Button from '../UI/Button.svelte';

    const dispatch = createEventDispatcher();

    export let meetups;
    let favsOnly = false;

    function setFilter(event) {
        favsOnly = event.detail === 1;
    }

    //! Reactive statement
    $: filteredMeetups = favsOnly
        ? meetups.filter(meet => meet.isFavorite)
        : meetups;
</script>

<style>
    #meetups {
        width: 100%;
        display: grid;
        grid-template-columns: 1fr;
        grid-gap: 1rem;
    }

    #meetup-controls {
        display: flex;
        justify-content: space-between;
        margin: 1rem;
    }

    #no-meetups {
        margin: 1rem;
        color: #fff;
    }

    @media (min-width: 768px) {
        #meetups {
            grid-template-columns: repeat(2, 1fr);
        }
    }
</style>

<section id="meetup-controls">
    <MeetupFilter on:select={setFilter} />
    <Button on:click={() => dispatch('add')}>New Meetup</Button>
</section>
{#if filteredMeetups.length === 0}
    <p id="no-meetups">No meetups found. Go ahead and add some</p>
{/if}
<section id="meetups">
    {#each filteredMeetups as meetup (meetup.id)}
        <div transition:scale animate:flip={{ duration: 300 }}>
            <MeetupItem
                id={meetup.id}
                title={meetup.title}
                subtitle={meetup.subtitle}
                description={meetup.description}
                imageUrl={meetup.imageUrl}
                email={meetup.contactEmail}
                address={meetup.address}
                isFav={meetup.isFavorite}
                on:togglefavorite
                on:showdetails
                on:edit />
        </div>
    {/each}
</section>
