# 114
.
import javax.swing.*;
import javax.swing.border.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.*;
import java.sql.*;
public class LibraryManager extends JFrame {
 private Connection conn;
 private JTextField bookIdField, titleField, authorField, categoryField, searchField;
 private JTable table;
 private DefaultTableModel model;
 private JTextField userField;
 private JPasswordField passField;
 public static void main(String[] args) {
 SwingUtilities.invokeLater(LibraryManager::new); // -------------------- LOGIN WINDOW --------------------
 private void showLoginUI() {
 JFrame login = new JFrame("MySQL Connection");
 login.setSize(420, 270);
 login.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
 login.setLocationRelativeTo(null);
 login.getContentPane().setBackground(new Color(245, 249, 255));
 login.setLayout(new BorderLayout());
 JLabel heading = new JLabel("Library Manager Login", SwingConstants.CENTER);
 heading.setFont(new Font("Poppins", Font.BOLD, 22));
 heading.setForeground(new Color(30, 60, 120));
 login.add(heading, BorderLayout.NORTH);
 JPanel panel = new JPanel(new GridBagLayout());
 panel.setOpaque(false);
 GridBagConstraints gbc = new GridBagConstraints();
 gbc.insets = new Insets(12, 12, 12, 12);
 gbc.fill = GridBagConstraints.HORIZONTAL;
 userField = new JTextField(15);
 passField = new JPasswordField(15);
 JButton connectBtn = new JButton("Connect →");
 styleButton(connectBtn, new Color(0, 123, 255));
 gbc.gridx = 0; gbc.gridy = 0; panel.add(new JLabel("Username:"), gbc);
 gbc.gridx = 1; panel.add(userField, gbc);
 gbc.gridx = 0; gbc.gridy = 1; panel.add(new JLabel("Password:"), gbc);
 gbc.gridx = 1; panel.add(passField, gbc);
 gbc.gridx = 1; gbc.gridy = 2; panel.add(connectBtn, gbc); login.add(panel, BorderLayout.CENTER);
 connectBtn.addActionListener(e -> {
 String user = userField.getText().trim();
 String pass = new String(passField.getPassword());
 connectToDB(login, user, pass);
 });
 login.setVisible(true);
 }
 private void connectToDB(JFrame login, String user, String pass) {
 try {
 String url = "jdbc:mysql://localhost:3306/librarydb";
 Class.forName("com.mysql.cj.jdbc.Driver");
 conn = DriverManager.getConnection(url, user, pass);
 JOptionPane.showMessageDialog(login, "Connected Successfully!");
 login.dispose();
 showMainUI();
 } catch (Exception ex) {
 JOptionPane.showMessageDialog(login, "Connection Failed:\n" + ex.getMessage());
 }
 }
 // -------------------- MAIN DASHBOARD --------------------
 private void showMainUI() {
 setTitle("Library Book Management System");
 setSize(1000, 600);
 setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
 setLocationRelativeTo(null); JPanel bgPanel = new JPanel() {
 protected void paintComponent(Graphics g) {
 super.paintComponent(g);
 Graphics2D g2 = (Graphics2D) g;
 GradientPaint gp = new GradientPaint(0, 0, new Color(215, 234, 255),
 0, getHeight(), new Color(180, 200, 255));
 g2.setPaint(gp);
 g2.fillRect(0, 0, getWidth(), getHeight());
 }
 };
 bgPanel.setLayout(new BorderLayout(10, 10));
 bgPanel.setBorder(BorderFactory.createEmptyBorder(15, 15, 15, 15));
 JLabel title = new JLabel("Library Book Management Dashboard", SwingConstants.CENTER);
 title.setFont(new Font("Poppins", Font.BOLD, 26));
 title.setForeground(new Color(25, 55, 125));
 bgPanel.add(title, BorderLayout.NORTH);
 model = new DefaultTableModel(new String[]{"Book ID", "Title", "Author", "Category"}, 0);
 table = new JTable(model);
 table.setSelectionBackground(new Color(102, 255, 102));
 table.setRowHeight(25);
 table.setFont(new Font("Segoe UI", Font.PLAIN, 14));
 table.getTableHeader().setFont(new Font("Segoe UI", Font.BOLD, 14));
 table.getTableHeader().setBackground(new Color(80, 120, 200));
 table.getTableHeader().setForeground(Color.WHITE);
 JScrollPane sp = new JScrollPane(table);
 sp.setBorder(new LineBorder(new Color(140, 160, 220), 2, true)); // Form Section
 JPanel form = new JPanel(new GridBagLayout());
 form.setOpaque(false);
 GridBagConstraints gbc = new GridBagConstraints();
 gbc.insets = new Insets(8, 8, 8, 8);
 gbc.fill = GridBagConstraints.HORIZONTAL;
 bookIdField = new JTextField(10);
 titleField = new JTextField(15);
 authorField = new JTextField(15);
 categoryField = new JTextField(15);
 gbc.gridx = 0; gbc.gridy = 0; form.add(new JLabel("Book ID:"), gbc);
 gbc.gridx = 1; form.add(bookIdField, gbc);
 gbc.gridx = 0; gbc.gridy = 1; form.add(new JLabel("Title:"), gbc);
 gbc.gridx = 1; form.add(titleField, gbc);
 gbc.gridx = 0; gbc.gridy = 2; form.add(new JLabel("Author:"), gbc);
 gbc.gridx = 1; form.add(authorField, gbc);
 gbc.gridx = 0; gbc.gridy = 3; form.add(new JLabel("Category:"), gbc);
 gbc.gridx = 1; form.add(categoryField, gbc);
 JButton addBtn = new JButton("Add");
 JButton updateBtn = new JButton("✏ Update");
 JButton deleteBtn = new JButton("🗑 Delete");
 JButton clearBtn = new JButton("Clear");
 JButton refreshBtn = new JButton("Refresh");
 styleButton(addBtn, new Color(0, 153, 76));
 styleButton(updateBtn, new Color(255, 153, 51));
 styleButton(deleteBtn, new Color(230, 57, 70));
 styleButton(clearBtn, new Color(102, 102, 255)); styleButton(refreshBtn, new Color(70, 130, 180));
 gbc.gridx = 0; gbc.gridy = 4; form.add(addBtn, gbc);
 gbc.gridx = 1; form.add(updateBtn, gbc);
 gbc.gridx = 0; gbc.gridy = 5; form.add(deleteBtn, gbc);
 gbc.gridx = 1; form.add(clearBtn, gbc);
 gbc.gridx = 1; gbc.gridy = 6; form.add(refreshBtn, gbc);
 JPanel searchPanel = new JPanel(new FlowLayout(FlowLayout.RIGHT));
 searchPanel.setOpaque(false);
 searchField = new JTextField(10);
 JButton searchBtn = new JButton("Search");
 styleButton(searchBtn, new Color(51, 102, 255));
 searchPanel.add(new JLabel("Book ID: "));
 searchPanel.add(searchField);
 searchPanel.add(searchBtn);
 bgPanel.add(searchPanel, BorderLayout.SOUTH);
 bgPanel.add(form, BorderLayout.WEST);
 bgPanel.add(sp, BorderLayout.CENTER);
 add(bgPanel);
 setVisible(true);
 addBtn.addActionListener(e -> addBook());
 updateBtn.addActionListener(e -> updateBook());
 deleteBtn.addActionListener(e -> deleteBook());
 clearBtn.addActionListener(e -> clearFields());
 refreshBtn.addActionListener(e -> loadBooks());
 searchBtn.addActionListener(e -> searchBook()); loadBooks();
 }
 // -------------------- CRUD FUNCTIONS --------------------
 private void addBook() {
 try {
 int bookId = Integer.parseInt(bookIdField.getText());
 String title = titleField.getText().trim();
 String author = authorField.getText().trim();
 String category = categoryField.getText().trim();
 PreparedStatement ps = conn.prepareStatement("INSERT INTO book VALUES (?,?,?,?)");
 ps.setInt(1, bookId);
 ps.setString(2, title);
 ps.setString(3, author);
 ps.setString(4, category);
 ps.executeUpdate();
 JOptionPane.showMessageDialog(this, "Book Added!");
 loadBooks();
 clearFields();
 } catch (Exception e) {
 JOptionPane.showMessageDialog(this, "⚠ Error: " + e.getMessage());
 }
 }
 private void updateBook() {
 try {
 int bookId = Integer.parseInt(bookIdField.getText());
 String title = titleField.getText().trim();
 String author = authorField.getText().trim();
 String category = categoryField.getText().trim(); PreparedStatement ps = conn.prepareStatement(
 "UPDATE book SET title=?, author=?, category=? WHERE book_id=?");
 ps.setString(1, title);
 ps.setString(2, author);
 ps.setString(3, category);
 ps.setInt(4, bookId);
 int rows = ps.executeUpdate();
 JOptionPane.showMessageDialog(this, rows > 0 ? "✏ Updated Successfully!" : "⚠ No Record 
Found!");
 loadBooks();
 clearFields();
 } catch (Exception e) {
 JOptionPane.showMessageDialog(this, "⚠ Update Error: " + e.getMessage());
 }
 }
 private void deleteBook() {
 try {
 int bookId = Integer.parseInt(bookIdField.getText());
 PreparedStatement ps = conn.prepareStatement("DELETE FROM book WHERE book_id=?");
 ps.setInt(1, bookId);
 int rows = ps.executeUpdate();
 JOptionPane.showMessageDialog(this, rows > 0 ? "🗑 Deleted Successfully!" : "⚠ No Record 
Found!");
 loadBooks();
 clearFields();
 } catch (Exception e) {
 JOptionPane.showMessageDialog(this, "⚠ Delete Error: " + e.getMessage());
 }
 } private void searchBook() {
 try {
 int bookId = Integer.parseInt(searchField.getText().trim());
 loadBooks();
 boolean found = false;
 int rowIndex = -1;
 for (int i = 0; i < model.getRowCount(); i++) {
 int currentId = (int) model.getValueAt(i, 0);
 if (currentId == bookId) {
 table.setRowSelectionInterval(i, i);
 table.scrollRectToVisible(table.getCellRect(i, 0, true));
 found = true;
 rowIndex = i;
 break;
 }
 }
 if (!found) {
 JOptionPane.showMessageDialog(this, "⚠ No Record Found!");
 } else {
 bookIdField.setText(String.valueOf(model.getValueAt(rowIndex, 0)));
 titleField.setText(String.valueOf(model.getValueAt(rowIndex, 1)));
 authorField.setText(String.valueOf(model.getValueAt(rowIndex, 2)));
 categoryField.setText(String.valueOf(model.getValueAt(rowIndex, 3)));
 }
 } catch (Exception e) {
 JOptionPane.showMessageDialog(this, "⚠ Search Error: " + e.getMessage());
 }
 } private void loadBooks() {
 try {
 model.setRowCount(0);
 Statement st = conn.createStatement();
 ResultSet rs = st.executeQuery("SELECT * FROM book");
 while (rs.next()) {
 model.addRow(new Object[]{
 rs.getInt(1), rs.getString(2), rs.getString(3), rs.getString(4)
 });
 }
 } catch (Exception e) {
 JOptionPane.showMessageDialog(this, "⚠ Load Error: " + e.getMessage());
 }
 }
 private void clearFields() {
 bookIdField.setText("");
 titleField.setText("");
 authorField.setText("");
 categoryField.setText("");
 searchField.setText("");
 }
 private void styleButton(JButton btn, Color bg) {
 btn.setBackground(bg);
 btn.setForeground(Color.white);
 btn.setFocusPainted(false);
 btn.setFont(new Font("Segoe UI", Font.BOLD, 13));
 btn.setBorder(new RoundedBorder(10));
 btn.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));
 } static class RoundedBorder implements Border {
 private int radius;
 RoundedBorder(int radius) { this.radius = radius; }
 public Insets getBorderInsets(Component c) { return new Insets(this.radius+1, this.radius+1, 
this.radius+2, this.radius); }
 public boolean isBorderOpaque() { return false; }
 public void paintBorder(Component c, Graphics g, int x, int y, int width, int height) {
 g.drawRoundRect(x, y, width-1, height-1, radius, radius);
 }
 }
}
