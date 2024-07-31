<?php
header('Content-Type: application/json');
$username = $_POST['username'];
$email = $_POST['email'];
$password = password_hash($_POST['password'], PASSWORD_BCRYPT);

$conn = new mysqli('localhost', 'root', '', 'foodies_paradise');

if ($conn->connect_error) {
    die('Connection failed: ' . $conn->connect_error);
}

$stmt = $conn->prepare('SELECT id FROM users WHERE username = ? OR email = ?');
$stmt->bind_param('ss', $username, $email);
$stmt->execute();
$stmt->store_result();

if ($stmt->num_rows > 0) {
    echo json_encode(['success' => false, 'message' => 'Username or email already taken.']);
    $stmt->close();
    $conn->close();
    exit();
}

$stmt->close();
$stmt = $conn->prepare('INSERT INTO users (username, email, password) VALUES (?, ?, ?)');
$stmt->bind_param('sss', $username, $email, $password);

if ($stmt->execute()) {
    echo json_encode(['success' => true, 'message' => 'Registration successful.']);
} else {
    echo json_encode(['success' => false, 'message' => 'Registration failed.']);
}

$stmt->close();
$conn->close();
?>
