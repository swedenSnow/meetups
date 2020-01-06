<script>
    import meetups from './Meetups/meetups-store.js';
    import Header from './UI/Header.svelte';
    import MeetupGrid from './Meetups/MeetupGrid.svelte';
    import MeetupDetail from './Meetups/MeetupDetail.svelte';
    import TextInput from './UI/TextInput.svelte';
    import Button from './UI/Button.svelte';
    import EditForm from './Meetups/EditMeetup.svelte';

    let loadedMeetups = meetups;
    // let meetups = [
    //     {
    //         id: 'm1',
    //         title: 'Coding Bootcamp',
    //         subtitle: 'Learn to code in 2 hours',
    //         description:
    //             'In this meetup, we will have some experts that teach you how to code!',
    //         imageUrl:
    //             'https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG/800px-Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG',
    //         address: '27th Nerd Road, 32523 New York',
    //         contactEmail: 'code@test.com',
    //         isFavorite: false,
    //     },
    //     {
    //         id: 'm2',
    //         title: 'Swim Together',
    //         subtitle: "Let's go for some swimming",
    //         description: 'We will simply swim some rounds!',
    //         imageUrl:
    //             'https://upload.wikimedia.org/wikipedia/commons/thumb/6/69/Olympic_swimming_pool_%28Tbilisi%29.jpg/800px-Olympic_swimming_pool_%28Tbilisi%29.jpg',
    //         address: '27th Nerd Road, 32523 New York',
    //         contactEmail: 'swim@test.com',
    //         isFavorite: false,
    //     },
    // ];

    let editMode = null;
    let page = 'overview';
    let pageData = {};
    let editedId;

    function savedMeetup(event) {
        editMode = null;
        editedId = null;
    }

    function cancelEdit() {
        editMode = null;
        editedId = null;
    }

    // function toggleFavorite(event) {
    //     const id = event.detail;
    //     loadedMeetups.toggleFavorite(id);
    // }

    function showdetails(event) {
        page = 'details';
        pageData.id = event.detail;
    }

    function closeDetails() {
        page = 'overview';
        pageData = {};
    }

    function startEdit(event) {
        editMode = 'edit';
        editedId = event.detail;
    }
</script>

<style>
    main {
        margin-top: 5rem;
    }
</style>

<Header />
<main>
    {#if page === 'overview'}
        <!-- <Button on:click={() => (editMode = !editMode)}>Make New</Button> -->
        <!-- If editMode was set to false would be easy to toggle... -->
        {#if editMode === 'edit'}
            <EditForm
                id={editedId}
                on:save={savedMeetup}
                on:cancel={cancelEdit} />
        {/if}

        <MeetupGrid
            meetups={$loadedMeetups}
            on:showdetails={showdetails}
            on:edit={startEdit}
            on:add={() => (editMode = 'edit')} />
    {:else}
        <MeetupDetail id={pageData.id} on:close={closeDetails} />
    {/if}

</main>
