<template>
  <table>
    <caption>Food description</caption>

    <tr>
        <th>ELEMENT</th>
        <th>QUANTITY</th>
    </tr>
    <tr>
        <td>Calories</td>
        <td></td>
    </tr>
    <tr>
        <td>Carbohydrates(g)</td>
        <td></td>
    </tr>
    <tr>
        <td>Cholesterol(mg)</td>
        <td></td>
    </tr>
    <tr>
        <td>Saturated fat(g)</td>
        <td></td>
    </tr>
    <tr>
        <td>Total fat(g)</td>
        <td></td>
    </tr>
    <tr>
        <td>Fiber(g)</td>
        <td></td>
    </tr>
    <tr>
        <td>Potassium(g)</td>
        <td></td>
    </tr>
    <tr>
        <td>Protein(g)</td>
        <td></td>
    </tr>
    <tr>
        <td>Serving size(g)</td>
        <td></td>
    </tr>
    <tr>
        <td>Sodium(g)</td>
        <td></td>
    </tr>
    <tr>
        <td>Sugar(g)</td>
        <td></td>
    </tr>
  </table>  
</template>

<style scoped>
    table {
    table-layout: fixed;
    margin: 25px 0;
    font-size: 0.9em;
    width: 20%;
    border-collapse: collapse;
    border: 3px solid black;
    }

    thead th:nth-child(1) {
    width: 30%;
    }

    thead th:nth-child(2) {
    width: 20%;
    }

    thead th:nth-child(3) {
    width: 15%;
    }

    thead th:nth-child(4) {
    width: 35%;
    }

    th,
    td {
    padding: 20px;
    }

    .content-table thread tr{
        background-color: #009879
    }

    .table-cell
    {
        display: table-cell;
        width: 10%;
        text-align: center;
        padding: 4px 0;
        border-right: 1px solid #d6d4d4;
        vertical-align: middle;

    }
</style>