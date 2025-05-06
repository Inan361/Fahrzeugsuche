# Fahrzeugsuche
Schulprojekt
<?php
$conn = new mysqli("localhost", "root", "", "cardekho_db");
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$query = "SELECT * FROM cars WHERE 1=1";

if (!empty($_GET['fuel'])) {
    $fuel = $conn->real_escape_string($_GET['fuel']);
    $query .= " AND fuel = '$fuel'";
}
if (!empty($_GET['transmission'])) {
    $transmission = $conn->real_escape_string($_GET['transmission']);
    $query .= " AND transmission = '$transmission'";
}
if (!empty($_GET['year_min']) && !empty($_GET['year_max'])) {
    $min = (int)$_GET['year_min'];
    $max = (int)$_GET['year_max'];
    $query .= " AND year BETWEEN $min AND $max";
}

$result = $conn->query($query);
?>

<!DOCTYPE html>
<html>
<head>
    <title>Auto-Suche</title>
</head>
<body>
    <h1>Fahrzeugsuche</h1>
    <form method="GET">
        Kraftstoff:
        <select name="fuel">
            <option value="">Alle</option>
            <option value="Petrol">Benzin</option>
            <option value="Diesel">Diesel</option>
            <option value="CNG">CNG</option>
        </select>

        Getriebe:
        <select name="transmission">
            <option value="">Alle</option>
            <option value="Manual">Manuell</option>
            <option value="Automatic">Automatik</option>
        </select>

        Baujahr:
        <input type="number" name="year_min" placeholder="Min" />
        -
        <input type="number" name="year_max" placeholder="Max" />

        <input type="submit" value="Suchen" />
    </form>

    <h2>Ergebnisse:</h2>
    <table border="1">
        <tr>
            <th>Name</th>
            <th>Jahr</th>
            <th>Preis</th>
            <th>KM</th>
            <th>Kraftstoff</th>
            <th>Verkäufer</th>
            <th>Getriebe</th>
            <th>Besitzer</th>
        </tr>
        <?php while($row = $result->fetch_assoc()): ?>
            <tr>
                <td><?= htmlspecialchars($row['name']) ?></td>
                <td><?= $row['year'] ?></td>
                <td><?= $row['selling_price'] ?></td>
                <td><?= $row['km_driven'] ?></td>
                <td><?= $row['fuel'] ?></td>
                <td><?= $row['seller_type'] ?></td>
                <td><?= $row['transmission'] ?></td>
                <td><?= $row['owner'] ?></td>
            </tr>
        <?php endwhile; ?>
    </table>
</body>
</html>
