<script>
    import meetups from './Meetups/meetups-store.js';
    import Header from './UI/Header.svelte';
    import MeetupGrid from './Meetups/MeetupGrid.svelte';
    import MeetupDetail from './Meetups/MeetupDetail.svelte';
    import TextInput from './UI/TextInput.svelte';
    import Button from './UI/Button.svelte';
    import EditForm from './Meetups/EditMeetup.svelte';
    import LoadingSpinner from './UI/LoadingSpinner.svelte';
    import Error from './UI/Error.svelte';

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

    let loadedMeetups = meetups;
    let editMode = null;
    let page = 'overview';
    let pageData = {};
    let editedId;
    let isLoading = true;
    let error;

    // let y;
    // $: console.log(y);

    fetch('https://svelte-course-6d25d.firebaseio.com/meetups.json')
        .then(res => {
            if (!res.ok) {
                throw new Error(
                    'Fetching meetups failed, please try again later!'
                );
            }
            return res.json();
        })
        .then(data => {
            const loadedMeetups = [];
            for (const key in data) {
                loadedMeetups.push({
                    ...data[key],
                    id: key,
                });
            }
            setTimeout(() => {
                isLoading = false;
                meetups.setMeetups(loadedMeetups.reverse());
            }, 1000);
        })
        .catch(err => {
            error = err;
            isLoading = false;
            console.log(err);
        });

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
    function clearError() {
        error = null;
    }
</script>

<style>
    main {
        margin-top: 5rem;
    }
</style>

{#if error}
    <Error message={error.message} on:cancel={clearError} />
{/if}

<!-- <svelte:window on:keydown={() => alert('Hey keydown')} /> -->
<!-- <svelte:window bind:scrollY={y} /> -->
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
        {#if isLoading}
            <LoadingSpinner />
        {:else}
            <MeetupGrid
                meetups={$loadedMeetups}
                on:showdetails={showdetails}
                on:edit={startEdit}
                on:add={() => (editMode = 'edit')} />
        {/if}
    {:else}
        <MeetupDetail id={pageData.id} on:close={closeDetails} />
    {/if}

</main>
