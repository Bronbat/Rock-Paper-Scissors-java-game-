# Rock-Paper-Scissors-java-game-
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class RockPaperScissorsGame extends JPanel {
    private JTextField player1NameTextField;
    private JTextField player2NameTextField;
    private JLabel roundNumberLabel;
    private JRadioButton[] roundNumberButtons;
    private JButton startButton;

    private String player1Name;
    private String player2Name;
    private int totalRounds;

    public RockPaperScissorsGame() {
        setLayout(new BorderLayout());

        // Creating components
        JPanel inputPanel = createInputPanel();
        JPanel playPanel = createPlayPanel();

        // Adding components to the main panel
        add(inputPanel, BorderLayout.NORTH);
        add(playPanel, BorderLayout.CENTER);
    }

    private JPanel createInputPanel() {
        JPanel inputPanel = new JPanel();
        inputPanel.setLayout(new GridLayout(4, 2));

        JLabel player1NameLabel = new JLabel("Player 1 Name: ");
        player1NameTextField = new JTextField(15);

        JLabel player2NameLabel = new JLabel("Player 2 Name: ");
        player2NameTextField = new JTextField(15);

        roundNumberLabel = new JLabel("Select the number of rounds (1-10): ");
        JPanel roundNumberPanel = createRoundNumberPanel();

        startButton = new JButton("Start");

        // Adding components to the input panel
        inputPanel.add(player1NameLabel);
        inputPanel.add(player1NameTextField);
        inputPanel.add(player2NameLabel);
        inputPanel.add(player2NameTextField);
        inputPanel.add(roundNumberLabel);
        inputPanel.add(roundNumberPanel);
        inputPanel.add(startButton);

        // Registering ActionListener for the start button
        startButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                getPlayerNames();
                getRoundNumber();
                playGame();
            }
        });

        return inputPanel;
    }

    private JPanel createRoundNumberPanel() {
        JPanel roundNumberPanel = new JPanel();
        roundNumberButtons = new JRadioButton[10];

        ButtonGroup buttonGroup = new ButtonGroup();
        for (int i = 0; i < roundNumberButtons.length; i++) {
            roundNumberButtons[i] = new JRadioButton(Integer.toString(i + 1));
            buttonGroup.add(roundNumberButtons[i]);
            roundNumberPanel.add(roundNumberButtons[i]);
        }

        roundNumberButtons[0].setSelected(true); // Default selection

        return roundNumberPanel;
    }

    private JPanel createPlayPanel() {
        JPanel playPanel = new JPanel();
        playPanel.setLayout(new GridLayout(1, 2));

        JPanel player1Panel = createPlayerPanel("Player 1");
        JPanel player2Panel = createPlayerPanel("Player 2");

        playPanel.add(player1Panel);
        playPanel.add(player2Panel);

        return playPanel;
    }

    private JPanel createPlayerPanel(String playerName) {
        JPanel playerPanel = new JPanel();
        playerPanel.setLayout(new BorderLayout());

        JLabel nameLabel = new JLabel(playerName);
        JButton rockButton = createChoiceButton(playerName, "rock");
        JButton paperButton = createChoiceButton(playerName, "paper");
        JButton scissorsButton = createChoiceButton(playerName, "scissors");

        playerPanel.add(nameLabel, BorderLayout.NORTH);
        playerPanel.add(rockButton, BorderLayout.WEST);
        playerPanel.add(paperButton, BorderLayout.CENTER);
        playerPanel.add(scissorsButton, BorderLayout.EAST);

        return playerPanel;
    }

    private JButton createChoiceButton(String playerName, String choice) {
        JButton button = new JButton(choice);
        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String result = playerName + " chose: " + choice;
                JOptionPane.showMessageDialog(null, result, "Choice", JOptionPane.INFORMATION_MESSAGE);
                playRound(playerName, choice);
            }
        });
        return button;
    }

    private void getPlayerNames() {
        player1Name = player1NameTextField.getText();
        player2Name = player2NameTextField.getText();
    }

    private void getRoundNumber() {
        for (int i = 0; i < roundNumberButtons.length; i++) {
            if (roundNumberButtons[i].isSelected()) {
                totalRounds = i + 1;
                break;
            }
        }
    }

    private void playRound(String playerName, String choice) {
        String otherPlayerName = (playerName.equals("Player 1")) ? "Player 2" : "Player 1";

        String otherPlayerChoice = getPlayerChoice(otherPlayerName);
        String roundResult = determineRoundResult(playerName, choice, otherPlayerName, otherPlayerChoice);

        JOptionPane.showMessageDialog(null, "Round Result:\n" + roundResult, "Round Result", JOptionPane.INFORMATION_MESSAGE);
    }

    private String getPlayerChoice(String playerName) {
        String choice = "";
        boolean validChoice = false;

        while (!validChoice) {
            choice = JOptionPane.showInputDialog(null, playerName + ", enter your choice (rock, paper, scissors):");
            if (choice != null && (choice.equals("rock") || choice.equals("paper") || choice.equals("scissors"))) {
                validChoice = true;
            } else {
                JOptionPane.showMessageDialog(null, "Invalid choice! Please enter 'rock', 'paper', or 'scissors'.", "Invalid Choice", JOptionPane.WARNING_MESSAGE);
            }
        }

        return choice;
    }

    private String determineRoundResult(String player1Name, String player1Choice, String player2Name, String player2Choice) {
        if (player1Choice.equals(player2Choice)) {
            return "It's a tie!";
        } else if ((player1Choice.equals("rock") && player2Choice.equals("scissors")) ||
                (player1Choice.equals("scissors") && player2Choice.equals("paper")) ||
                (player1Choice.equals("paper") && player2Choice.equals("rock"))) {
            return player1Name + " wins";
        } else {
            return player2Name + " wins";
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                JFrame frame = new JFrame("Rock Paper Scissors Game");
                frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
                frame.setSize(600, 400);
                frame.setLocationRelativeTo(null);

                RockPaperScissorsGame gamePanel = new RockPaperScissorsGame();
                frame.getContentPane().add(gamePanel);

                frame.setVisible(true);
            }
        });
    }
}
