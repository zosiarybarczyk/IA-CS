import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.net.URL;
/**
 * Write a description of class konto2 here.
 *
 * @author (your name)
 * @version (a version number or a date)
 */
public class konto2 extends JFrame
{
    private static final String FILE_NAME = "loginData.txt";
    private static final String EYE_OPEN_ICON = "/Resource/open.jpg";  // Replace with your eye open icon file path
    private static final String EYE_CLOSED_ICON = "/Resource/closed.jpg"; // Replace with your eye closed icon file path
    private static final Color HOVER_COLOR = new Color(173, 216, 230); // Light blue
    
    
    // Components for the GUI
    private JTextField usernameField;
    private JPasswordField passwordField;
    private JLabel togglePasswordVisibilityLabel;
    private JButton createAccountButton;
    private JButton loginButton;
    private JButton forgotPasswordButton; // Forgot Password button
    private boolean isPasswordVisible = false; // Track visibility state
    private JComboBox<String> roleComboBox; // Role selection

     // Constructor to set up the GUI
    public konto2() {
        setTitle("Login System");
        setSize(400, 250); // Adjusted size
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // Center the window

        // Create a panel with BorderLayout
        JPanel panel = new JPanel();
        panel.setLayout(new BorderLayout());

        // Create a panel for input fields and buttons
        JPanel inputPanel = new JPanel();
        inputPanel.setLayout(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.fill = GridBagConstraints.HORIZONTAL;
        gbc.insets = new Insets(10, 10, 10, 10);

        // Username
        gbc.gridx = 0;
        gbc.gridy = 0;
        inputPanel.add(new JLabel("Username:"), gbc);

        gbc.gridx = 1;
        gbc.gridy = 0;
        usernameField = new JTextField(20);
        inputPanel.add(usernameField, gbc);

        // Password
        gbc.gridx = 0;
        gbc.gridy = 1;
        inputPanel.add(new JLabel("Password:"), gbc);

        gbc.gridx = 1;
        gbc.gridy = 1;
        JPanel passwordPanel = new JPanel(new BorderLayout());
        passwordField = new JPasswordField(20);
        passwordPanel.add(passwordField, BorderLayout.CENTER);

        // Load and scale icons
        ImageIcon eyeOpenIcon = scaleIcon(new ImageIcon(getClass().getResource(EYE_CLOSED_ICON)), 20, 20);
        ImageIcon eyeClosedIcon = scaleIcon(new ImageIcon(getClass().getResource(EYE_OPEN_ICON)), 20, 20);


        
        togglePasswordVisibilityLabel = new JLabel(eyeClosedIcon);
        togglePasswordVisibilityLabel.setCursor(new Cursor(Cursor.HAND_CURSOR));
        togglePasswordVisibilityLabel.addMouseListener(new MouseAdapter() {
            public void mouseClicked(MouseEvent e) {
                togglePasswordVisibility();
            }
        });

        passwordPanel.add(togglePasswordVisibilityLabel, BorderLayout.EAST);
        inputPanel.add(passwordPanel, gbc);
        
        // Role selection
        gbc.gridx = 0;
        gbc.gridy = 3;
        inputPanel.add(new JLabel("Role:"), gbc);

        gbc.gridx = 1;
        roleComboBox = new JComboBox<>(new String[]{"user", "admin"});
        inputPanel.add(roleComboBox, gbc);


        
         // Forgot Password Button
        gbc.gridx = 1;
        gbc.gridy = 2;
        gbc.anchor = GridBagConstraints.LINE_END;
        forgotPasswordButton = new JButton("Forgot Password");
        forgotPasswordButton.setFont(new Font("Arial", Font.PLAIN, 12)); // Smaller font size
        forgotPasswordButton.setBorderPainted(false);
        forgotPasswordButton.setContentAreaFilled(false);
        forgotPasswordButton.setFocusPainted(false);
        forgotPasswordButton.setForeground(Color.BLACK); // Set text color
        
        // Add mouse listener for hover effect
        forgotPasswordButton.addMouseListener(new MouseAdapter() {
            public void mouseEntered(MouseEvent e) {
                forgotPasswordButton.setForeground(Color.BLUE); // Change text color on hover
                forgotPasswordButton.setBackground(HOVER_COLOR); // Change background color on hover
                forgotPasswordButton.setOpaque(true); // Make background visible
            }
            public void mouseExited(MouseEvent e) {
                forgotPasswordButton.setForeground(Color.BLACK); // Reset text color
                forgotPasswordButton.setBackground(null); // Reset background color
                forgotPasswordButton.setOpaque(false); // Make background invisible
            }
        });
        
        forgotPasswordButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                forgotPassword();
            }
        });
        inputPanel.add(forgotPasswordButton, gbc);

        // Add inputPanel to the center of the main panel
        panel.add(inputPanel, BorderLayout.CENTER);

        // Buttons
        JPanel buttonPanel = new JPanel();
        buttonPanel.setLayout(new FlowLayout());

        createAccountButton = new JButton("Create Account");
        createAccountButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                createAccount();
            }
        });
        buttonPanel.add(createAccountButton);

        loginButton = new JButton("Login");
        loginButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                login();
            }
        });
        buttonPanel.add(loginButton);
        
        //forgotPasswordButton = new JButton("Forgot Password");
        //forgotPasswordButton.addActionListener(new ActionListener() {
            //public void actionPerformed(ActionEvent e) {
               // forgotPassword();
            //}
       // });
       // buttonPanel.add(forgotPasswordButton);

        // Add buttonPanel to the south of the main panel
        panel.add(buttonPanel, BorderLayout.SOUTH);

        // Add the main panel to the frame
        add(panel);
        
        setAlwaysOnTop(true);  // Ensures the window stays on top
        toFront();  // Brings 
        setVisible(true);
        requestFocus();
    }
     // Scale the icon to the specified width and height
    private ImageIcon scaleIcon(ImageIcon icon, int width, int height) {
        Image image = icon.getImage();
        Image scaledImage = image.getScaledInstance(width, height, Image.SCALE_SMOOTH);
        return new ImageIcon(scaledImage);}

     // Toggle password visibility
    private void togglePasswordVisibility() {
        if (isPasswordVisible) {
            // Hide password
            passwordField.setEchoChar('*');
            togglePasswordVisibilityLabel.setIcon(scaleIcon(new ImageIcon(getClass().getResource(EYE_CLOSED_ICON)), 20, 20));
        } else {
            // Show password
            passwordField.setEchoChar((char) 0);
            togglePasswordVisibilityLabel.setIcon(scaleIcon(new ImageIcon(getClass().getResource(EYE_OPEN_ICON)), 20, 20));
        }
        isPasswordVisible = !isPasswordVisible;
    }

    // Method to create a new account and save to file
    private void createAccount() {
        String username = usernameField.getText();
        String password = new String(passwordField.getPassword());
        String role = (String) roleComboBox.getSelectedItem();

        if (!username.isEmpty() && !password.isEmpty()) {
            saveAccount(username, password, role);
            JOptionPane.showMessageDialog(this, "Account created successfully!");
        } else {
            JOptionPane.showMessageDialog(this, "Please enter both username and password.");
        }
    }
    
     // Validate login by checking the username, password, and role in the file
private String validateLogin(String username, String password) {
    try (BufferedReader reader = new BufferedReader(new FileReader(FILE_NAME))) {
        String line;
        while ((line = reader.readLine()) != null) {
            if (line.contains("Username: " + username)) {
                String passwordLine = reader.readLine(); // Read the next line for the password
                if (passwordLine != null && passwordLine.contains("Password: " + password)) {
                    String roleLine = reader.readLine(); // Read the next line for the role
                    if (roleLine != null && roleLine.contains("Role: ")) {
                        return roleLine.split(": ")[1]; // Return the role (e.g., "admin" or "user")
                    }
                }
            }
        }
    } catch (IOException e) {
        JOptionPane.showMessageDialog(this, "An error occurred while validating the login.");
    }
    return null; // No match found
}

    // Method to log in with the saved account
   private void login() {
    String username = usernameField.getText();
    String password = new String(passwordField.getPassword());

    String role = validateLogin(username, password); // Now returns the role
    if (role != null) {
        JOptionPane.showMessageDialog(this, "Login successful as " + role + "!");
        dispose();
        openMainScreen(role); // Pass role to openMainScreen
    } else {
        JOptionPane.showMessageDialog(this, "Login error! Invalid username or password.");
    }
}
     private void forgotPassword() {
        String username = JOptionPane.showInputDialog(this, "Enter your username to reset your password:");

        if (username != null && !username.isEmpty()) {
            // Check if the username exists
            if (usernameExists(username)) {
                String newPassword = JOptionPane.showInputDialog(this, "Enter a new password:");
                if (newPassword != null && !newPassword.isEmpty()) {
                    updatePassword(username, newPassword);
                    JOptionPane.showMessageDialog(this, "Password reset successfully!");
                } else {
                    JOptionPane.showMessageDialog(this, "New password cannot be empty.");
                }
            } else {
                JOptionPane.showMessageDialog(this, "Username not found.");
            }
        } else {
            JOptionPane.showMessageDialog(this, "Username cannot be empty.");
        }
    }

    private boolean usernameExists(String username) {
        try (BufferedReader reader = new BufferedReader(new FileReader(FILE_NAME))) {
            String line;
            while ((line = reader.readLine()) != null) {
                if (line.contains("Username: " + username)) {
                    return true;
                }
            }
        } catch (IOException e) {
            JOptionPane.showMessageDialog(this, "An error occurred while checking the username.");
        }
        return false;
    }

    private void updatePassword(String username, String newPassword) {
        File tempFile = new File("temp.txt");
        try (BufferedReader reader = new BufferedReader(new FileReader(FILE_NAME));
             BufferedWriter writer = new BufferedWriter(new FileWriter(tempFile))) {
            String line;
            boolean found = false;
            while ((line = reader.readLine()) != null) {
                if (line.contains("Username: " + username)) {
                    writer.write(line + "\n");
                    line = reader.readLine(); // Read the password line
                    if (line != null) {
                        writer.write("   Password: " + newPassword + "\n");
                        found = true;
                    }
                } else {
                    writer.write(line + "\n");
                }
            }
            if (!found) {
                JOptionPane.showMessageDialog(this, "Username not found.");
            }
        } catch (IOException e) {
            JOptionPane.showMessageDialog(this, "An error occurred while updating the password.");
        }
        tempFile.renameTo(new File(FILE_NAME));
    }

    // Save account to a file in numbered format, including a role
private void saveAccount(String username, String password, String role) {
    try (BufferedWriter writer = new BufferedWriter(new FileWriter(FILE_NAME, true))) {
        int accountNumber = countAccounts() + 1; // Increment account number
        writer.write(accountNumber + ". Username: " + username + "\n");
        writer.write("   Password: " + password + "\n");
        writer.write("   Role: " + role + "\n"); // Add the role
    } catch (IOException e) {
        JOptionPane.showMessageDialog(this, "An error occurred while saving the account.");
    }
}

    // Count the number of accounts stored in the file
    private int countAccounts() {
        int count = 0;
        try (BufferedReader reader = new BufferedReader(new FileReader(FILE_NAME))) {
            String line;
            while ((line = reader.readLine()) != null) {
                if (line.matches("\\d+\\. Username:.*")) { // Check for lines that start with the account number
                    count++;
                }
            }
        } catch (IOException e) {
            JOptionPane.showMessageDialog(this, "An error occurred while counting the accounts.");
        }
        return count;
    }


   // Method to open the main screen based on the role
private void openMainScreen(String role) {
    JFrame mainScreen = new JFrame("Main Screen");
    mainScreen.setSize(600, 400); // Larger size for the main screen
    mainScreen.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    mainScreen.setLayout(new BorderLayout());

    JLabel welcomeLabel = new JLabel("Welcome to the Main Screen! Role: " + role, JLabel.CENTER);
    mainScreen.add(welcomeLabel, BorderLayout.NORTH);

    JPanel rolePanel = new JPanel();
    rolePanel.setLayout(new FlowLayout());

    // Options for "admin"
    if (role.equals("admin")) {
        JButton adminPanelButton = new JButton("Admin Panel");
        adminPanelButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                showAdminPanel();
            }
        });
        rolePanel.add(adminPanelButton);
        
        // Add more admin-specific functionalities as needed
        JButton manageUsersButton = new JButton("Manage Users");
        manageUsersButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                JOptionPane.showMessageDialog(mainScreen, "Manage users clicked!");
                // Add your manage users logic here
            }
        });
        rolePanel.add(manageUsersButton);

    } 
    // Options for "user"
    else if (role.equals("user")) {
        JButton viewProfileButton = new JButton("View Profile");
        viewProfileButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                JOptionPane.showMessageDialog(mainScreen, "Viewing profile!");
                // Add logic for viewing the user profile
            }
        });
        rolePanel.add(viewProfileButton);

        JButton settingsButton = new JButton("Settings");
        settingsButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                JOptionPane.showMessageDialog(mainScreen, "Settings clicked!");
                // Add logic for user settings
            }
        });
        rolePanel.add(settingsButton);
    }

    // Add the rolePanel to the mainScreen
    mainScreen.add(rolePanel, BorderLayout.CENTER);

    // Center the main screen window
    mainScreen.setLocationRelativeTo(null);
    mainScreen.setVisible(true);
}

// Example of an Admin Panel method
private void showAdminPanel() {
    JFrame adminPanel = new JFrame("Admin Panel");
    adminPanel.setSize(400, 300);
    adminPanel.setLayout(new FlowLayout());

    JLabel adminLabel = new JLabel("Admin Functions:");
    adminPanel.add(adminLabel);

    // Add admin-specific functionalities here (e.g., buttons for managing users)
    JButton manageUsers = new JButton("Manage Users");
    adminPanel.add(manageUsers);

    // Add more buttons as needed for admin-specific tasks

    adminPanel.setLocationRelativeTo(null);
    adminPanel.setVisible(true);
}

    // Main method to run the program
public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                new konto2();
            }
        });
    }
}


