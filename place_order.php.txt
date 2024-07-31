<?php
if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $name = $_POST['name'];
    $address = $_POST['address'];
    $phone = $_POST['phone'];
    $orderDetails = $_POST['order-details'];

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

    $sql = "INSERT INTO orders (name, address, phone, order_details) VALUES ('$name', '$address', '$phone', '$orderDetails')";

    if ($conn->query($sql) === TRUE) {
        echo "<script>
            alert('Your order has been placed successfully!');
            window.location.href = 'payment.html';
        </script>";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }

    $conn->close();
}
?>

