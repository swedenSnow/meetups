<script>
    import { createEventDispatcher } from 'svelte';
    import TextInput from '../UI/TextInput.svelte';
    import Button from '../UI/Button.svelte';
    import Modal from '../UI/Modal.svelte';
    //! Helpers
    import {
        isEmpty,
        isValidEmail,
        isValidUrl,
    } from '../helpers/validation.js';

    let title = '';
    // let titleValid = false; SVelte does create these for you!!!
    let subtitle = '';
    // let subtitleValid = false;
    let address = '';
    // let addressValid = false;
    let email = '';
    // let emailValid = false;
    let description = '';
    // let descriptionValid = false;
    let imageUrl = '';
    // let imageUrlValid = false;

    let formIsValid = false;

    const dispatch = createEventDispatcher();

    $: titleValid = !isEmpty(title);
    $: subtitleValid = !isEmpty(subtitle);
    $: addressValid = !isEmpty(address);
    $: descriptionValid = !isEmpty(description);
    $: imageUrlValid = isValidUrl(imageUrl);
    $: emailValid = isValidEmail(email);
    $: formIsValid =
        titleValid &&
        subtitleValid &&
        addressValid &&
        descriptionValid &&
        imageUrlValid &&
        emailValid;

    function submitForm() {
        dispatch('save', {
            title: title,
            subtitle: subtitle,
            address: address,
            email: email,
            description: description,
            imageUrl: imageUrl,
        });
    }

    function cancel() {
        dispatch('cancel');
    }
</script>

<style>
    form {
        max-width: 100%;
    }
</style>

<Modal title="Edit Meetup Data" on:cancel>
    <form on:submit|preventDefault={submitForm}>
        <TextInput
            id="title"
            label="Title"
            valid={titleValid}
            errorMsg="Please enter a valid title"
            value={title}
            on:input={event => (title = event.target.value)} />
        <TextInput
            id="subtitle"
            label="Subtitle"
            valid={subtitleValid}
            errorMsg="Please enter a valid subtitle"
            value={subtitle}
            on:input={event => (subtitle = event.target.value)} />
        <TextInput
            id="address"
            label="Address"
            valid={addressValid}
            errorMsg="Please enter a valid address"
            value={address}
            on:input={event => (address = event.target.value)} />
        <TextInput
            id="imageUrl"
            label="Image URL"
            value={imageUrl}
            valid={imageUrlValid}
            errorMsg="Please enter a valid URL"
            on:input={event => (imageUrl = event.target.value)} />
        <TextInput
            id="email"
            label="E-Mail"
            type="email"
            valid={emailValid}
            errorMsg="Please enter a valid email"
            value={email}
            on:input={event => (email = event.target.value)} />
        <TextInput
            id="description"
            label="Description"
            controlType="textarea"
            valid={descriptionValid}
            errorMsg="Please enter a valid description"
            bind:value={description} />
    </form>
    <div slot="footer">
        <Button type="button" mode="outline" on:click={cancel}>Cancel</Button>
        <Button type="button" on:click={submitForm} disabled={!formIsValid}>
            Save
        </Button>
    </div>
</Modal>
