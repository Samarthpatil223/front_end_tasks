# front_end_tasks

#task_07

<select id="monthDropdown">
    <option value="March">March</option>
    <!-- Options for Jan to Dec -->
</select>
<input type="text" id="searchBox" placeholder="Search transaction">
<table>
    <thead>
        <tr>
            <th>ID</th><th>Title</th><th>Description</th><th>Price</th><th>Category</th><th>Sold</th>
        </tr>
    </thead>
    <tbody id="transactionTableBody"></tbody>
</table>
<button id="prevPage">Previous</button>
<button id="nextPage">Next</button>

<script>
let page = 1;
const perPage = 10;

document.getElementById('searchBox').addEventListener('input', fetchTransactions);
document.getElementById('monthDropdown').addEventListener('change', fetchTransactions);

function fetchTransactions() {
    const search = document.getElementById('searchBox').value;
    const month = document.getElementById('monthDropdown').value;

    fetch(`/transactions?search=${search}&page=${page}&perPage=${perPage}&month=${month}`)
        .then(res => res.json())
        .then(data => {
            const tbody = document.getElementById('transactionTableBody');
            tbody.innerHTML = '';
            data.forEach(item => {
                const row = `<tr><td>${item.id}</td><td>${item.title}</td><td>${item.description}</td><td>${item.price}</td><td>${item.category}</td><td>${item.sold}</td></tr>`;
                tbody.innerHTML += row;
            });
        });
}

document.getElementById('prevPage').addEventListener('click', () => {
    if (page > 1) {
        page--;
        fetchTransactions();
    }
});
document.getElementById('nextPage').addEventListener('click', () => {
    page++;
    fetchTransactions();
});

fetchTransactions();
</script>

#task_08

<div>
    <h3>Statistics</h3>
    <p>Total Sales: <span id="totalSales"></span></p>
    <p>Total Sold Items: <span id="soldItems"></span></p>
    <p>Total Unsold Items: <span id="unsoldItems"></span></p>
</div>

<script>
function fetchStatistics() {
    const month = document.getElementById('monthDropdown').value;

    fetch(`/statistics?month=${month}`)
        .then(res => res.json())
        .then(data => {
            document.getElementById('totalSales').textContent = data.totalSales;
            document.getElementById('soldItems').textContent = data.soldItems;
            document.getElementById('unsoldItems').textContent = data.unsoldItems;
        });
}

document.getElementById('monthDropdown').addEventListener('change', fetchStatistics);
fetchStatistics();
</script>

#task_09

<canvas id="priceRangeChart"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
function fetchPriceRangeData() {
    const month = document.getElementById('monthDropdown').value;

    fetch(`/price-range?month=${month}`)
        .then(res => res.json())
        .then(data => {
            const ctx = document.getElementById('priceRangeChart').getContext('2d');
            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: Object.keys(data),
                    datasets: [{
                        label: 'Number of Items',
                        data: Object.values(data),
                        backgroundColor: 'rgba(54, 162, 235, 0.2)',
                        borderColor: 'rgba(54, 162, 235, 1)',
                        borderWidth: 1
                    }]
                },
                options: {
                    scales: {
                        y: { beginAtZero: true }
                    }
                }
            });
        });
}

document.getElementById('monthDropdown').addEventListener('change', fetchPriceRangeData);
fetchPriceRangeData();
</script>
