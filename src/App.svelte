<script>
    import {fade} from 'svelte/transition'
    import {onMount} from 'svelte'
    import Catalog from './components/Catalog.svelte'
    import NavPanel from './components/NavPanel.svelte'
    import WishList from './components/WishList.svelte'
    import Cart from './components/Cart.svelte'
    import {userStore} from './user'

    let showWishList = false
    let showCart = false

    let latestFavorite = ''
    let latestBasket = ''

    function addFavoriteNumber(event) {
        latestFavorite = event.detail.title
        if (!$userStore.favoriteList.includes(latestFavorite)) {
            $userStore.favoriteList.push(latestFavorite)
            $userStore.favoriteNumber = $userStore.favoriteList.length
        }
        localStorage.setItem('chicago-user', JSON.stringify($userStore))
    }

    function addBasketNumber(event) {
        latestBasket = event.detail.title
        if (!$userStore.basketList.includes(latestBasket)) {
            $userStore.basketList.push(latestBasket)
            $userStore.basketNumber = $userStore.basketList.length
        }
        localStorage.setItem('chicago-user', JSON.stringify($userStore))
    }

    // Every component has a lifecycle that starts when it is created, and ends when it is destroyed.
    // There are a handful of functions that allow you to run code at key moments during that lifecycle.
    // Svelte specific function: runs after the component is first rendered to the DOM
    onMount(async () => {
        const storedJSON = localStorage.getItem('chicago-user')
        const stored = JSON.parse(storedJSON)
        Object.keys($userStore).forEach(key => {
            if (stored.hasOwnProperty(key)) {
                $userStore[key] = stored[key]
            }
        })
    })

    function navPanelHeartClickHandler() {
        showWishList = !showWishList
        showCart = false
    }

    function wishListCloseHandler() {
        showWishList = false
    }

    function navPanelCartClickHandler() {
        showCart = !showCart
        showWishList = false
    }

    function cartCloseHandler() {
        showCart = false
    }
</script>

<main>
    <NavPanel on:heart-click={navPanelHeartClickHandler} on:basket-click={navPanelCartClickHandler}/>

    <!--	reactivity: component watches parameter values changing-->
    <Catalog favorites={$userStore.favoriteList}
             basket={$userStore.basketList}
             on:framebox-heart={addFavoriteNumber}
             on:framebox-basket={addBasketNumber}
    />

    {#if showWishList === true}
        <div transition:fade="{{delay: 250, duration: 200}}">
            <WishList on:close={wishListCloseHandler}/>
        </div>

    {/if}

    {#if showCart === true}
        <div transition:fade="{{delay: 250, duration: 200}}">
            <Cart on:close={cartCloseHandler}/>
        </div>
    {/if}


</main>

<style>
    main {
        position: relative;
        text-align: center;
        padding: 0;
        /*max-width: 240px;*/
        width: 100%;
        margin: 0 0;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
</style>