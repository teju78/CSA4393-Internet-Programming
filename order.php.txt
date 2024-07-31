<?php
header('Content-Type: application/json');
session_start();

if (!isset($_SESSION['user_id'])) {
    echo json_encode(['success' => false, 'message' => 'User not logged in.']);
    exit;
}

$user_id = $_SESSION['user_id'];
$item_id = isset($_POST['item_id']) ? intval($_POST['item_id']) : 0;
$quantity = isset($_POST['quantity']) ? intval($_POST['quantity']) : 1;

$conn = new mysqli('localhost', 'root', '', 'foodies_paradise');

if ($conn->connect_error) {
    die(json_encode(['success' => false, 'message' => 'Connection failed: ' . $conn->connect_error]));
}

$stmt = $conn->prepare('SELECT id, quantity FROM cart WHERE user_id = ? AND item_id = ?');
$stmt->bind_param('ii', $user_id, $item_id);
$stmt->execute();
$stmt->bind_result($cart_id, $existing_quantity);
$stmt->fetch();
$stmt->close();

if ($cart_id) {
    $new_quantity = $existing_quantity + $quantity;
    $stmt = $conn->prepare('UPDATE cart SET quantity = ? WHERE id = ?');
    $stmt->bind_param('ii', $new_quantity, $cart_id);
} else {
    $stmt = $conn->prepare('INSERT INTO cart (user_id, item_id, quantity) VALUES (?, ?, ?)');
    $stmt->bind_param('iii', $user_id, $item_id, $quantity);
}

if ($stmt->execute()) {
    echo json_encode(['success' => true, 'message' => 'Item added to cart successfully.']);
} else {
    echo json_encode(['success' => false, 'message' => 'Failed to add item to cart.']);
}

$stmt->close();
$conn->close();
?>
