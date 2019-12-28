<script>
    import Header from './UI/Header.svelte';
    import MeetupGrid from './Meetups/MeetupGrid.svelte';
    import TextInput from './UI/TextInput.svelte';
    import Button from './UI/Button.svelte';
    import EditForm from './Meetups/EditMeetup.svelte';

    let meetups = [
        {
            id: 'm1',
            title: 'Coding Bootcamp',
            subtitle: 'Learn to code in 2 hours',
            description:
                'In this meetup, we will have some experts that teach you how to code!',
            imageUrl:
                'https://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG/800px-Caffe_Nero_coffee_bar%2C_High_St%2C_Sutton%2C_Surrey%2C_Greater_London.JPG',
            address: '27th Nerd Road, 32523 New York',
            contactEmail: 'code@test.com',
            isFavorite: false,
        },
        {
            id: 'm2',
            title: 'Swim Together',
            subtitle: "Let's go for some swimming",
            description: 'We will simply swim some rounds!',
            imageUrl:
                'https://upload.wikimedia.org/wikipedia/commons/thumb/6/69/Olympic_swimming_pool_%28Tbilisi%29.jpg/800px-Olympic_swimming_pool_%28Tbilisi%29.jpg',
            address: '27th Nerd Road, 32523 New York',
            contactEmail: 'swim@test.com',
            isFavorite: false,
        },
    ];

    let editMode = null;

    function addMeetup(event) {
        const e = event.detail;
        // console.table(event.detail);
        const newMeetup = {
            id: Math.random().toString(),
            title: e.title,
            subtitle: e.subtitle,
            description: e.description,
            imageUrl: e.imageUrl,
            contactEmail: e.email,
            address: e.address,
        };

        // meetups.push(newMeetup); // DOES NOT WORK!
        meetups = [newMeetup, ...meetups];

        editMode = null;
    }

    function cancelEdit() {
        editMode = null;
    }

    function toggleFavorite(event) {
        const id = event.detail;
        const updatedMeetup = { ...meetups.find(m => m.id === id) };
        updatedMeetup.isFavorite = !updatedMeetup.isFavorite;
        const meetupIndex = meetups.findIndex(m => m.id === id);
        const updatedMeetups = [...meetups];
        updatedMeetups[meetupIndex] = updatedMeetup;
        meetups = updatedMeetups;
    }
</script>

<style>
    main {
        margin-top: 5rem;
    }

    .meetup-form-container {
        margin: 1rem;
    }
</style>

<Header />
<main>
    <div class="meetup-form-container">
        <!-- If editMode was set to false would be easy to toggle... -->
        <!-- <Button on:click={() => (editMode = !editMode)}>Make New</Button> -->
        <Button on:click={() => (editMode = 'add')}>Make New</Button>
    </div>
    {#if editMode == 'add'}
        <EditForm on:save={addMeetup} on:cancel={cancelEdit} />
    {/if}
    <MeetupGrid {meetups} on:togglefavorite={toggleFavorite} />
</main>
