<script>
    import { createEventDispatcher } from 'svelte';

    export let title;
    export let heartSelected = false;
    export let basketSelected = false;

    // Svelte: special feature to sent events to parent component
    const dispatch = createEventDispatcher();

    function heartClick() {
        // send to parent-component event "heart-click" with object as a param (event.detail)
        dispatch('heart-click', {title: title});
        heartSelected = true
    }

    function basketClick() {
        dispatch('basket-click', {title: title})
        basketSelected = true
    }
</script>

<div class="frame">
    {title}
    <div class="toolbar">
        <i class="fas fa-search-plus"></i>

        <!-- bind function heartClick to mouse click event on icon -->
        <i class="fas fa-heart {heartSelected ? 'active-favorite' : ''}"
           on:click={heartClick}>
        </i>

        <i class="fas fa-cart-plus {basketSelected ? 'active-basket' : ''}"
           on:click={basketClick}>
        </i>

    </div>
</div>

<style>
    .frame {
        background-color: #dddddd;
        font-size: 3rem;
        height: 100%;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        position: relative;
    }
    .toolbar {
        background-color: white;
        color: #666666;
        /*height: 3rem;*/
        font-size: 2rem;
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        padding: 0.25rem 0.5rem;
        opacity: 0.5;
    }
    .toolbar>i {
        margin: 0 0.8rem;
    }
    .toolbar:hover {
        opacity: 1;
    }
    .active-favorite {
        color: red;
    }
    .active-basket {
        color:darkblue;
    }
</style>