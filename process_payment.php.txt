<?php
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $cardName = $_POST['card-name'];
    $cardNumber = $_POST['card-number'];
    $expiryDate = $_POST['expiry-date'];
    $cvv = $_POST['cvv'];

    // Database connection
    $servername = "localhost";
    $username = "root"; // default XAMPP username
    $password = ""; // default XAMPP password
    $dbname = "your_database_name";

    $conn = new mysqli($servername, $username, $password, $dbname);

    // Check connection
    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }

    $sql = "INSERT INTO payments (card_name, card_number, expiry_date, cvv) VALUES ('$cardName', '$cardNumber', '$expiryDate', '$cvv')";

    if ($conn->query($sql) === TRUE) {
        echo "<script>
            alert('Payment successful!');
            window.location.href = 'confirmation.html';
        </script>";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }

    $conn->close();
}
?>
