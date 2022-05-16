<script>
    import materialStore from './material-store.js';

    let materials = [];

    materialStore.subscribe(items => {
        materials = items;
    });

    $: total = materials.reduce((prev, next) => {
        prev += next.price;
        return prev;
    }, 0);

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
</style>
<h1>{total}</h1>
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
            <tr>
                <td>{material.name}</td>
                <td>{formatter.format(material.price)}</td>
                <td><i class="far fa-trash-alt"></i></td>
            </tr>
        {/each}
        <tr class="total">
            <td>Total</td>
            <td colspan="2">{formatter.format(total)}</td>
        </tr>
    </tbody>
</table>