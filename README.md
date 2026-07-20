import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import javax.swing.table.JTableHeader;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.util.HashMap;
import java.util.Map;

/**
 * Online Reservation System with Dynamic Cyber-Dark Theme & Hover Effects.
 * Single-file layout designed for easy import into IntelliJ IDEA.
 */
public class OnlineReservationSystem extends JFrame {
    private CardLayout cardLayout;
    private JPanel mainContainer;
    
    // Core Central Database Map: Links PNR String -> TicketDetails object
    private Map<String, TicketDetails> centralDatabase;
    private int pnrCounter = 1024351; 

    public OnlineReservationSystem() {
        setTitle("Online Reservation System - Cyber Terminal");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(1100, 700);
        setLocationRelativeTo(null); 
        
        centralDatabase = new HashMap<>();
        seedInitialRecords();

        cardLayout = new CardLayout();
        mainContainer = new JPanel(cardLayout);

        LoginPanel loginPanel = new LoginPanel(this);
        DashboardPanel dashboardPanel = new DashboardPanel(this);

        mainContainer.add(loginPanel, "LOGIN");
        mainContainer.add(dashboardPanel, "DASHBOARD");

        add(mainContainer);
        cardLayout.show(mainContainer, "LOGIN");
    }

    public void loginSuccess() {
        cardLayout.show(mainContainer, "DASHBOARD");
    }

    public void logout() {
        cardLayout.show(mainContainer, "LOGIN");
    }

    public Map<String, TicketDetails> getDatabase() {
        return centralDatabase;
    }

    public String generateNextPNR() {
        return "PNR" + (pnrCounter++);
    }

    private void seedInitialRecords() {
        centralDatabase.put("PNR1024349", new TicketDetails("PNR1024349", "Alex Morgan", "12301", "Rajdhani Express", "AC First Class", "2026-08-12", "New Delhi", "Mumbai"));
        centralDatabase.put("PNR1024350", new TicketDetails("PNR1024350", "Sarah Jenkins", "12002", "Shatabdi Express", "Executive Chair Car", "2026-08-15", "New Delhi", "Bhopal"));
    }

    public static class TicketDetails {
        public String pnr, passengerName, trainNo, trainName, classType, date, from, to;

        public TicketDetails(String pnr, String passengerName, String trainNo, String trainName, String classType, String date, String from, String to) {
            this.pnr = pnr;
            this.passengerName = passengerName;
            this.trainNo = trainNo;
            this.trainName = trainName;
            this.classType = classType;
            this.date = date;
            this.from = from;
            this.to = to;
        }
    }

    public static void main(String[] args) {
        try {
            UIManager.setLookAndFeel(UIManager.getSystemLookAndFeelClassName());
        } catch (Exception ignored) {}

        SwingUtilities.invokeLater(() -> {
            new OnlineReservationSystem().setVisible(true);
        });
    }
}

// =========================================================
// 1. MODULE: LOGIN FORM (Fixed Highlighted Purple Button)
// =========================================================
class LoginPanel extends JPanel {
    public LoginPanel(OnlineReservationSystem app) {
        setLayout(new GridBagLayout());
        setBackground(new Color(18, 24, 36)); // Midnight Obsidian Background

        // Interactive Glowing Card Panel
        JPanel formContainer = new JPanel();
        formContainer.setLayout(new BoxLayout(formContainer, BoxLayout.Y_AXIS));
        formContainer.setBackground(new Color(26, 35, 51)); // Deep Navy Card
        formContainer.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(new Color(138, 43, 226), 2), // Neon Purple Border Accent
                BorderFactory.createEmptyBorder(40, 50, 40, 50)
        ));

        // Module Title Label
        JLabel mainHeading = new JLabel("ONLINE RESERVATION SYSTEM");
        mainHeading.setFont(new Font("Segoe UI", Font.BOLD, 22));
        mainHeading.setForeground(new Color(240, 244, 248)); // Crisp White text
        mainHeading.setAlignmentX(Component.CENTER_ALIGNMENT);

        // Input Grid Fields setup
        JPanel inputGrid = new JPanel(new GridLayout(2, 2, 10, 15));
        inputGrid.setBackground(new Color(26, 35, 51));

        JLabel userLabel = new JLabel("Username ID:");
        userLabel.setFont(new Font("Segoe UI", Font.BOLD, 13));
        userLabel.setForeground(new Color(174, 186, 201));
        
        JTextField userField = new JTextField(12);
        userField.setBackground(new Color(36, 47, 66));
        userField.setForeground(Color.WHITE);
        userField.setCaretColor(Color.WHITE);
        userField.setBorder(BorderFactory.createLineBorder(new Color(58, 71, 94)));

        JLabel passLabel = new JLabel("Password:");
        passLabel.setFont(new Font("Segoe UI", Font.BOLD, 13));
        passLabel.setForeground(new Color(174, 186, 201));
        
        JPasswordField passField = new JPasswordField(12);
        passField.setBackground(new Color(36, 47, 66));
        passField.setForeground(Color.WHITE);
        passField.setCaretColor(Color.WHITE);
        passField.setBorder(BorderFactory.createLineBorder(new Color(58, 71, 94)));

        inputGrid.add(userLabel);
        inputGrid.add(userField);
        inputGrid.add(passLabel);
        inputGrid.add(passField);

        // Interactive Highlighted Login Button (Fixed OS Overlay Bug)
        JButton loginBtn = new JButton("Login Terminal");
        loginBtn.setFont(new Font("Segoe UI", Font.BOLD, 14));
        loginBtn.setBackground(new Color(138, 43, 226)); // Blue Violet / Neon Purple Accent
        loginBtn.setForeground(Color.WHITE);
        loginBtn.setFocusPainted(false);
        loginBtn.setBorderPainted(false);       
        loginBtn.setContentAreaFilled(true);    
        loginBtn.setOpaque(true);               
        loginBtn.setAlignmentX(Component.CENTER_ALIGNMENT);
        loginBtn.setPreferredSize(new Dimension(180, 42));
        loginBtn.setMaximumSize(new Dimension(180, 42));
        loginBtn.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));

        // Hover Effect for Login Button
        loginBtn.addMouseListener(new java.awt.event.MouseAdapter() {
            public void mouseEntered(java.awt.event.MouseEvent evt) {
                loginBtn.setBackground(new Color(178, 76, 255)); // Bright Neon Purple Glow
            }
            public void mouseExited(java.awt.event.MouseEvent evt) {
                loginBtn.setBackground(new Color(138, 43, 226)); 
            }
        });

        formContainer.add(mainHeading);
        formContainer.add(Box.createVerticalStrut(30));
        formContainer.add(inputGrid);
        formContainer.add(Box.createVerticalStrut(25));
        formContainer.add(loginBtn);

        add(formContainer);

        loginBtn.addActionListener(e -> {
            app.loginSuccess();
        });
    }
}

// =========================================================
// 2. DASHBOARD HUB PANEL (Manages Navigation & Views)
// =========================================================
class DashboardPanel extends JPanel {
    private OnlineReservationSystem app;
    private JPanel contentArea;
    private CardLayout subCardLayout;
    
    // Central Repository Database for Trains
    private final Map<String, String[]> trainScheduleDatabase = Map.of(
        "12301", new String[]{"Rajdhani Express", "Daily", "New Delhi -> Kanpur -> Prayagraj -> Mumbai", "Active"},
        "12002", new String[]{"Shatabdi Express", "Mon, Tue, Thu, Fri", "New Delhi -> Mathura -> Agra -> Bhopal", "Active"},
        "12952", new String[]{"Mumbai Tejas Express", "Daily except Sunday", "Mumbai -> Vadodara -> Ratlam -> New Delhi", "Active"},
        "22436", new String[]{"Vande Bharat Express", "Tue, Wed, Fri, Sat, Sun", "New Delhi -> Ambala Cantt -> Chandigarh", "Active"}
    );

    private DefaultTableModel tableModel;
    private JTable dbTable;

    public DashboardPanel(OnlineReservationSystem app) {
        this.app = app;
        setLayout(new BorderLayout());

        JPanel sidebar = new JPanel();
        sidebar.setLayout(new BoxLayout(sidebar, BoxLayout.Y_AXIS));
        sidebar.setBackground(new Color(18, 24, 36)); 
        sidebar.setPreferredSize(new Dimension(240, 0));
        sidebar.setBorder(BorderFactory.createEmptyBorder(25, 12, 25, 12));

        JLabel logo = new JLabel("⚡ CORE SYSTEM HUB");
        logo.setForeground(new Color(0, 191, 255)); 
        logo.setFont(new Font("Segoe UI", Font.BOLD, 16));
        logo.setAlignmentX(Component.CENTER_ALIGNMENT);
        sidebar.add(logo);
        sidebar.add(Box.createVerticalStrut(40));

        JButton navBook = createNavButton("📝 Reservation Form");
        JButton navCancel = createNavButton("❌ Cancellation Form");
        JButton navTrainDetails = createNavButton("🚆 Train Schedule Details");
        JButton navDatabase = createNavButton("📊 Central Database");
        JButton navLogout = createNavButton("🚪 Logout System");
        navLogout.setForeground(new Color(255, 107, 107));

        sidebar.add(navBook);
        sidebar.add(Box.createVerticalStrut(12));
        sidebar.add(navCancel);
        sidebar.add(Box.createVerticalStrut(12));
        sidebar.add(navTrainDetails);
        sidebar.add(Box.createVerticalStrut(12));
        sidebar.add(navDatabase);
        sidebar.add(Box.createVerticalGlue());
        sidebar.add(navLogout);

        subCardLayout = new CardLayout();
        contentArea = new JPanel(subCardLayout);
        contentArea.setBackground(new Color(26, 35, 51)); 

        JPanel bookView = buildBookView();
        JPanel cancelView = buildCancelView();
        JPanel trainDetailsView = buildTrainDetailsView();
        JPanel databaseView = buildDatabaseView();

        contentArea.add(bookView, "BOOK");
        contentArea.add(cancelView, "CANCEL");
        contentArea.add(trainDetailsView, "TRAIN_DETAILS");
        contentArea.add(databaseView, "DATABASE");

        add(sidebar, BorderLayout.WEST);
        add(contentArea, BorderLayout.CENTER);

        navBook.addActionListener(e -> subCardLayout.show(contentArea, "BOOK"));
        navCancel.addActionListener(e -> subCardLayout.show(contentArea, "CANCEL"));
        navTrainDetails.addActionListener(e -> subCardLayout.show(contentArea, "TRAIN_DETAILS"));
        navDatabase.addActionListener(e -> {
            refreshDatabaseTable();
            subCardLayout.show(contentArea, "DATABASE");
        });
        navLogout.addActionListener(e -> app.logout());
    }

    private JButton createNavButton(String text) {
        JButton btn = new JButton(text);
        btn.setMaximumSize(new Dimension(220, 42));
        btn.setFont(new Font("Segoe UI", Font.BOLD, 13));
        btn.setBackground(new Color(30, 41, 59)); 
        btn.setForeground(new Color(226, 232, 240));
        btn.setFocusPainted(false);
        btn.setBorderPainted(false);
        btn.setContentAreaFilled(true);
        btn.setOpaque(true);
        btn.setAlignmentX(Component.CENTER_ALIGNMENT);
        btn.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));
        
        btn.addMouseListener(new java.awt.event.MouseAdapter() {
            public void mouseEntered(java.awt.event.MouseEvent evt) {
                btn.setBackground(new Color(0, 119, 255)); 
                btn.setForeground(Color.WHITE);
            }
            public void mouseExited(java.awt.event.MouseEvent evt) {
                btn.setBackground(new Color(30, 41, 59));
                btn.setForeground(new Color(226, 232, 240));
            }
        });
        return btn;
    }

    private void styleDarkTextField(JTextField field) {
        field.setBackground(new Color(30, 41, 59));
        field.setForeground(Color.WHITE);
        field.setCaretColor(Color.WHITE);
        field.setFont(new Font("Segoe UI", Font.PLAIN, 13));
        field.setBorder(BorderFactory.createLineBorder(new Color(71, 85, 105)));
    }

    private void styleDarkLabel(JLabel label) {
        label.setForeground(new Color(226, 232, 240));
        label.setFont(new Font("Segoe UI", Font.BOLD, 13));
    }

    // =========================================================
    // 3. MODULE: RESERVATION SYSTEM
    // =========================================================
    private JPanel buildBookView() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(26, 35, 51));
        panel.setBorder(BorderFactory.createEmptyBorder(30, 40, 30, 40));

        JLabel title = new JLabel("Online Reservation Input Form");
        title.setFont(new Font("Segoe UI", Font.BOLD, 22));
        title.setForeground(Color.WHITE);
        panel.add(title, BorderLayout.NORTH);

        JPanel form = new JPanel(new GridLayout(8, 2, 15, 15));
        form.setBackground(new Color(26, 35, 51));
        form.setBorder(BorderFactory.createEmptyBorder(25, 0, 25, 0));

        JTextField nameField = new JTextField(); styleDarkTextField(nameField);
        JTextField trainNoField = new JTextField(); styleDarkTextField(trainNoField);
        JTextField trainNameField = new JTextField(); styleDarkTextField(trainNameField);
        trainNameField.setEditable(false);
        trainNameField.setBackground(new Color(21, 29, 43));
        trainNameField.setBorder(BorderFactory.createLineBorder(new Color(51, 65, 85)));
        
        trainNoField.addFocusListener(new java.awt.event.FocusAdapter() {
            public void focusLost(java.awt.event.FocusEvent evt) {
                String no = trainNoField.getText().trim();
                if (trainScheduleDatabase.containsKey(no)) {
                    trainNameField.setText(trainScheduleDatabase.get(no)[0]);
                } else {
                    trainNameField.setText("Unknown Train Variant");
                }
            }
        });

        JComboBox<String> classBox = new JComboBox<>(new String[]{"AC First Class", "AC 2 Tier", "Sleeper Class", "Executive Chair Car"});
        classBox.setBackground(new Color(30, 41, 59));
        classBox.setForeground(Color.WHITE);
        
        JTextField dateField = new JTextField("2026-08-20"); styleDarkTextField(dateField);
        JTextField fromField = new JTextField(); styleDarkTextField(fromField);
        JTextField toField = new JTextField(); styleDarkTextField(toField);

        JLabel l1 = new JLabel("Basic Details (Passenger Name):"); styleDarkLabel(l1); form.add(l1); form.add(nameField);
        JLabel l2 = new JLabel("Train Number (e.g. 12301, 12002):"); styleDarkLabel(l2); form.add(l2); form.add(trainNoField);
        JLabel l3 = new JLabel("Train Name (Automated Box):"); styleDarkLabel(l3); form.add(l3); form.add(trainNameField);
        JLabel l4 = new JLabel("Class Type Designation:"); styleDarkLabel(l4); form.add(l4); form.add(classBox);
        JLabel l5 = new JLabel("Date of Journey (YYYY-MM-DD):"); styleDarkLabel(l5); form.add(l5); form.add(dateField);
        JLabel l6 = new JLabel("From (Place):"); styleDarkLabel(l6); form.add(l6); form.add(fromField);
        JLabel l7 = new JLabel("To Destination:"); styleDarkLabel(l7); form.add(l7); form.add(toField);

        JButton insertBtn = new JButton("Insert Record");
        insertBtn.setFont(new Font("Segoe UI", Font.BOLD, 15));
        insertBtn.setBackground(new Color(0, 200, 83)); // Emerald Green
        insertBtn.setForeground(Color.WHITE);
        insertBtn.setFocusPainted(false);
        insertBtn.setBorderPainted(false);
        insertBtn.setContentAreaFilled(true);
        insertBtn.setOpaque(true);
        insertBtn.setPreferredSize(new Dimension(0, 45));
        insertBtn.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));
        
        insertBtn.addMouseListener(new java.awt.event.MouseAdapter() {
            public void mouseEntered(java.awt.event.MouseEvent evt) {
                insertBtn.setBackground(new Color(0, 230, 118)); 
            }
            public void mouseExited(java.awt.event.MouseEvent evt) {
                insertBtn.setBackground(new Color(0, 200, 83));
            }
        });

        insertBtn.addActionListener(e -> {
            if(nameField.getText().isBlank() || trainNoField.getText().isBlank() || fromField.getText().isBlank() || toField.getText().isBlank()) {
                JOptionPane.showMessageDialog(panel, "Error: Fill out all fields before committing data.", "Validation Fault", JOptionPane.WARNING_MESSAGE);
                return;
            }
            
            String assignedPnr = app.generateNextPNR();
            OnlineReservationSystem.TicketDetails newTicket = new OnlineReservationSystem.TicketDetails(
                    assignedPnr, nameField.getText().trim(), trainNoField.getText().trim(),
                    trainNameField.getText(), classBox.getSelectedItem().toString(),
                    dateField.getText().trim(), fromField.getText().trim(), toField.getText().trim()
            );
            
            app.getDatabase().put(assignedPnr, newTicket);
            JOptionPane.showMessageDialog(panel, "Successfully Inserted Record into Central Database!\nGenerated Key: " + assignedPnr, "Operation Complete", JOptionPane.INFORMATION_MESSAGE);
            
            nameField.setText(""); trainNoField.setText(""); trainNameField.setText("");
            fromField.setText(""); toField.setText("");
        });

        panel.add(form, BorderLayout.CENTER);
        panel.add(insertBtn, BorderLayout.SOUTH);

        return panel;
    }

    // =========================================================
    // 4. MODULE: CANCELLATION FORM
    // =========================================================
    private JPanel buildCancelView() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(26, 35, 51));
        panel.setBorder(BorderFactory.createEmptyBorder(30, 40, 30, 40));

        JLabel title = new JLabel("System Ticket Cancellation Module");
        title.setFont(new Font("Segoe UI", Font.BOLD, 22));
        title.setForeground(Color.WHITE);
        panel.add(title, BorderLayout.NORTH);

        JPanel centerContainer = new JPanel();
        centerContainer.setLayout(new BoxLayout(centerContainer, BoxLayout.Y_AXIS));
        centerContainer.setBackground(new Color(26, 35, 51));
        centerContainer.setBorder(BorderFactory.createEmptyBorder(20, 0, 20, 0));

        JPanel searchBar = new JPanel(new BorderLayout(15, 0));
        searchBar.setBackground(new Color(26, 35, 51));
        searchBar.setMaximumSize(new Dimension(Integer.MAX_VALUE, 40));
        
        JTextField searchField = new JTextField("Enter PNR Number here..."); styleDarkTextField(searchField);
        
        JButton fetchBtn = new JButton("Submit Query");
        fetchBtn.setBackground(new Color(255, 0, 127)); // Deep Neon Pink
        fetchBtn.setForeground(Color.WHITE);
        fetchBtn.setFont(new Font("Segoe UI", Font.BOLD, 13));
        fetchBtn.setFocusPainted(false);
        fetchBtn.setBorderPainted(false);
        fetchBtn.setContentAreaFilled(true);
        fetchBtn.setOpaque(true);
        fetchBtn.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));
        
        fetchBtn.addMouseListener(new java.awt.event.MouseAdapter() {
            public void mouseEntered(java.awt.event.MouseEvent evt) {
                fetchBtn.setBackground(new Color(255, 77, 166)); 
            }
            public void mouseExited(java.awt.event.MouseEvent evt) {
                fetchBtn.setBackground(new Color(255, 0, 127));
            }
        });
        
        searchBar.add(searchField, BorderLayout.CENTER);
        searchBar.add(fetchBtn, BorderLayout.EAST);
        centerContainer.add(searchBar);
        centerContainer.add(Box.createVerticalStrut(20));

        JTextArea summaryDisplay = new JTextArea();
        summaryDisplay.setEditable(false);
        summaryDisplay.setFont(new Font("Monospaced", Font.PLAIN, 14));
        summaryDisplay.setForeground(new Color(0, 230, 118)); 
        summaryDisplay.setBorder(BorderFactory.createLineBorder(new Color(71, 85, 105)));
        summaryDisplay.setBackground(new Color(18, 24, 36));
        centerContainer.add(new JScrollPane(summaryDisplay));

        JButton confirmCancelBtn = new JButton("OK - Confirm System Drop Execution");
        confirmCancelBtn.setFont(new Font("Segoe UI", Font.BOLD, 15));
        confirmCancelBtn.setBackground(new Color(229, 57, 53)); 
        confirmCancelBtn.setForeground(Color.WHITE);
        confirmCancelBtn.setFocusPainted(false);
        confirmCancelBtn.setBorderPainted(false);
        confirmCancelBtn.setContentAreaFilled(true);
        confirmCancelBtn.setOpaque(true);
        confirmCancelBtn.setPreferredSize(new Dimension(0, 45));
        confirmCancelBtn.setEnabled(false); 
        confirmCancelBtn.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));

        confirmCancelBtn.addMouseListener(new java.awt.event.MouseAdapter() {
            public void mouseEntered(java.awt.event.MouseEvent evt) {
                if(confirmCancelBtn.isEnabled()) confirmCancelBtn.setBackground(new Color(255, 23, 68)); 
            }
            public void mouseExited(java.awt.event.MouseEvent evt) {
                if(confirmCancelBtn.isEnabled()) confirmCancelBtn.setBackground(new Color(229, 57, 53));
            }
        });

        final OnlineReservationSystem.TicketDetails[] selectedTicketRef = {null};

        fetchBtn.addActionListener(e -> {
            String targetPnr = searchField.getText().trim();
            if(app.getDatabase().containsKey(targetPnr)) {
                selectedTicketRef[0] = app.getDatabase().get(targetPnr);
                summaryDisplay.setText(String.format(
                    "=====================================================\n" +
                    "           PNR REGISTRY LOGISTICAL ENVELOPE\n" +
                    "=====================================================\n" +
                    " » Passenger Name      : %s\n" +
                    " » Train Matrix Context: Number %s [%s]\n" +
                    " » Passenger Class Type: %s\n" +
                    " » Selected Date       : %s\n" +
                    " » Route Trajectory    : From %s to %s\n" +
                    "=====================================================\n" +
                    "CONFIRMATION ADVISORY: Press the OK button below to drop.",
                    selectedTicketRef[0].passengerName, selectedTicketRef[0].trainNo, selectedTicketRef[0].trainName,
                    selectedTicketRef[0].classType, selectedTicketRef[0].date, selectedTicketRef[0].from, selectedTicketRef[0].to
                ));
                confirmCancelBtn.setEnabled(true); 
            } else {
                selectedTicketRef[0] = null;
                summaryDisplay.setText("No information matches that PNR record sequence inside the database core cluster registry.");
                confirmCancelBtn.setEnabled(false);
            }
        });

        confirmCancelBtn.addActionListener((ActionEvent e) -> {
            if(selectedTicketRef[0] != null) {
                app.getDatabase().remove(selectedTicketRef[0].pnr);
                JOptionPane.showMessageDialog(panel, "System Drop Complete. Ticket reference dropped from central database memory.", "Database Updated", JOptionPane.INFORMATION_MESSAGE);
                summaryDisplay.setText("");
                searchField.setText("");
                confirmCancelBtn.setEnabled(false);
                selectedTicketRef[0] = null;
            }
        });

        panel.add(centerContainer, BorderLayout.CENTER);
        panel.add(confirmCancelBtn, BorderLayout.SOUTH);

        return panel;
    }

    // =========================================================
    // 5. MODULE: TRAIN SCHEDULE SEARCH ENGINE
    // =========================================================
    private JPanel buildTrainDetailsView() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(26, 35, 51));
        panel.setBorder(BorderFactory.createEmptyBorder(30, 40, 30, 40));

        JLabel title = new JLabel("🚆 Train Schedule Info Lookup");
        title.setFont(new Font("Segoe UI", Font.BOLD, 22));
        title.setForeground(Color.WHITE);
        panel.add(title, BorderLayout.NORTH);

        JPanel contentContainer = new JPanel();
        contentContainer.setLayout(new BoxLayout(contentContainer, BoxLayout.Y_AXIS));
        contentContainer.setBackground(new Color(26, 35, 51));
        contentContainer.setBorder(BorderFactory.createEmptyBorder(20, 0, 20, 0));

        JPanel inputPanel = new JPanel(new BorderLayout(15, 0));
        inputPanel.setBackground(new Color(26, 35, 51));
        inputPanel.setMaximumSize(new Dimension(Integer.MAX_VALUE, 40));

        JTextField trainSearchField = new JTextField("Try typing 12301, 12002, 12952, or 22436..."); styleDarkTextField(trainSearchField);
        
        JButton fetchTrainBtn = new JButton("Fetch Schedule Details");
        fetchTrainBtn.setBackground(new Color(0, 188, 212)); // Cyan Base
        fetchTrainBtn.setForeground(Color.WHITE);
        fetchTrainBtn.setFont(new Font("Segoe UI", Font.BOLD, 13));
        fetchTrainBtn.setFocusPainted(false);
        fetchTrainBtn.setBorderPainted(false);
        fetchTrainBtn.setContentAreaFilled(true);
        fetchTrainBtn.setOpaque(true);
        fetchTrainBtn.setCursor(Cursor.getPredefinedCursor(Cursor.HAND_CURSOR));

        fetchTrainBtn.addMouseListener(new java.awt.event.MouseAdapter() {
            public void mouseEntered(java.awt.event.MouseEvent evt) {
                fetchTrainBtn.setBackground(new Color(0, 229, 255)); 
                fetchTrainBtn.setForeground(Color.BLACK); 
            }
            public void mouseExited(java.awt.event.MouseEvent evt) {
                fetchTrainBtn.setBackground(new Color(0, 188, 212));
                fetchTrainBtn.setForeground(Color.WHITE);
            }
        });

        inputPanel.add(trainSearchField, BorderLayout.CENTER);
        inputPanel.add(fetchTrainBtn, BorderLayout.EAST);
        contentContainer.add(inputPanel);
        contentContainer.add(Box.createVerticalStrut(20)); // Fixed Typo Here!

        JTextArea scheduleDisplay = new JTextArea();
        scheduleDisplay.setEditable(false);
        scheduleDisplay.setFont(new Font("Monospaced", Font.PLAIN, 14));
        scheduleDisplay.setForeground(new Color(0, 191, 255)); 
        scheduleDisplay.setBorder(BorderFactory.createLineBorder(new Color(71, 85, 105)));
        scheduleDisplay.setBackground(new Color(18, 24, 36));
        contentContainer.add(new JScrollPane(scheduleDisplay));

        fetchTrainBtn.addActionListener(e -> {
            String queryNo = trainSearchField.getText().trim();
            if (trainScheduleDatabase.containsKey(queryNo)) {
                String[] details = trainScheduleDatabase.get(queryNo);
                scheduleDisplay.setText(String.format(
                    "=====================================================\n" +
                    "          CENTRAL RAILWAY NETWORK DATA MATRIX\n" +
                    "=====================================================\n" +
                    " • Train Number      : %s\n" +
                    " • Train Name        : %s\n" +
                    " • Running Days      : %s\n" +
                    " • Route Map Info    : %s\n" +
                    " • Operational Status: %s\n" +
                    "=====================================================\n" +
                    " System Check: Live monitoring feeds connection active.",
                    queryNo, details[0], details[1], details[2], details[3]
                ));
            } else {
                scheduleDisplay.setText(
                    "=====================================================\n" +
                    "               RECORD SEARCH ERROR\n" +
                    "=====================================================\n" +
                    " No database file records correspond to Train: " + queryNo + "\n" +
                    " Verify the index or try key references: 12301, 12002, 12952, 22436"
                );
            }
        });

        panel.add(contentContainer, BorderLayout.CENTER);
        return panel;
    }

    // =========================================================
    // 6. MODULE: CENTRAL DATABASE GRID VIEW
    // =========================================================
    private JPanel buildDatabaseView() {
        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(new Color(26, 35, 51));
        panel.setBorder(BorderFactory.createEmptyBorder(30, 40, 30, 40));

        JLabel title = new JLabel("Central Storage Vault Database Dashboard");
        title.setFont(new Font("Segoe UI", Font.BOLD, 22));
        title.setForeground(Color.WHITE);
        panel.add(title, BorderLayout.NORTH);

        String[] headers = {"PNR Key String", "Passenger Name", "Train No", "Train Name", "Class Type", "Date", "From", "To Target"};
        tableModel = new DefaultTableModel(headers, 0);
        dbTable = new JTable(tableModel);
        dbTable.setRowHeight(28);
        
        dbTable.setBackground(new Color(30, 41, 59));
        dbTable.setForeground(Color.WHITE);
        dbTable.setGridColor(new Color(51, 65, 85));
        dbTable.setFont(new Font("Segoe UI", Font.PLAIN, 13));
        
        JTableHeader header = dbTable.getTableHeader();
        header.setBackground(new Color(18, 24, 36));
        header.setForeground(new Color(0, 191, 255));
        header.setFont(new Font("Segoe UI", Font.BOLD, 13));
        
        JScrollPane scrollPane = new JScrollPane(dbTable);
        scrollPane.getViewport().setBackground(new Color(26, 35, 51));
        scrollPane.setBorder(BorderFactory.createLineBorder(new Color(51, 65, 85)));
        panel.add(scrollPane, BorderLayout.CENTER);

        return panel;
    }

    private void refreshDatabaseTable() {
        tableModel.setRowCount(0); 
        for (Map.Entry<String, OnlineReservationSystem.TicketDetails> entry : app.getDatabase().entrySet()) {
            OnlineReservationSystem.TicketDetails t = entry.getValue();
            tableModel.addRow(new Object[]{t.pnr, t.passengerName, t.trainNo, t.trainName, t.classType, t.date, t.from, t.to});
        }
    }
}
