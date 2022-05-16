<script>
    import { createEventDispatcher } from 'svelte';
    import materialStore from './material-store.js';

    const dispatch = createEventDispatcher();
    let materials = [];

    materialStore.subscribe(items => {
        materials = items;
    });

    $: total = materials.reduce((prev, next) => {
        prev += next.price;
        return prev;
    }, 0);

    function edit(id, name, price) {
        dispatch('edit', {id, name, price});
    }

    function remove(id) {
        materialStore.remove(id);
    }

    const formatter = new Intl.NumberFormat('de-DE', {
        style: 'currency',
        currency: 'EUR'
    });
</script>

<style>
    table {
        width: 100%;
    }
    .total td {
        font-weight: 900;
    }

    .item:hover {
        font-weight: 900;
        cursor: pointer;
    }
</style>

<table class="primary">
    <thead>
        <tr>
            <th>Material</th>
            <th>Price</th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        {#each materials as material (material.id)}
            <tr class="item" on:click={edit(material.id, material.name, material.price)}>
                <td>{material.name}</td>
                <td>{formatter.format(material.price)}</td>
                <td><i on:click|stopPropagation={remove(material.id)} class="far fa-trash-alt"></i></td>
            </tr>
        {/each}
        <tr class="total">
            <td>Total</td>
            <td colspan="2">{formatter.format(total)}</td>
        </tr>
    </tbody>
</table>