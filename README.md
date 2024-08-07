import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class CalorieCalculatorGUI extends JFrame implements ActionListener {

    private JTextField weightField, heightField, ageField;
    private JRadioButton maleRadioButton, femaleRadioButton;
    private JButton calculateButton, continueButton;
    private JLabel resultLabel, bmiLabel, itemsLabel, activitiesLabel;
    private JTextArea itemsTextArea, activitiesTextArea;

    public CalorieCalculatorGUI() {
        setTitle("Calorie Calculator");
        setSize(400, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(null);

        JLabel genderLabel = new JLabel("Select Gender:");
        genderLabel.setBounds(20, 20, 120, 20);
        add(genderLabel);

        maleRadioButton = new JRadioButton("Male");
        maleRadioButton.setBounds(150, 20, 80, 20);
        add(maleRadioButton);

        femaleRadioButton = new JRadioButton("Female");
        femaleRadioButton.setBounds(250, 20, 100, 20);
        add(femaleRadioButton);

        ButtonGroup genderGroup = new ButtonGroup();
        genderGroup.add(maleRadioButton);
        genderGroup.add(femaleRadioButton);

        JLabel weightLabel = new JLabel("Weight (kg):");
        weightLabel.setBounds(20, 60, 100, 20);
        add(weightLabel);

        weightField = new JTextField();
        weightField.setBounds(150, 60, 200, 20);
        add(weightField);

        JLabel heightLabel = new JLabel("Height (cm):");
        heightLabel.setBounds(20, 100, 100, 20);
        add(heightLabel);

        heightField = new JTextField();
        heightField.setBounds(150, 100, 200, 20);
        add(heightField);

        JLabel ageLabel = new JLabel("Age (years):");
        ageLabel.setBounds(20, 140, 100, 20);
        add(ageLabel);

        ageField = new JTextField();
        ageField.setBounds(150, 140, 200, 20);
        add(ageField);

        calculateButton = new JButton("Calculate");
        calculateButton.setBounds(20, 180, 100, 30);
        calculateButton.addActionListener(this);
        add(calculateButton);

        continueButton = new JButton("Continue");
        continueButton.setBounds(150, 180, 100, 30);
        continueButton.addActionListener(this);
        continueButton.setEnabled(false);
        add(continueButton);

        resultLabel = new JLabel();
        resultLabel.setBounds(20, 220, 400, 20);
        add(resultLabel);

        bmiLabel = new JLabel();
        bmiLabel.setBounds(20, 240, 400, 20);
        add(bmiLabel);

        itemsLabel = new JLabel("Items to eat:");
        itemsLabel.setBounds(20, 260, 200, 20);
        add(itemsLabel);

        itemsTextArea = new JTextArea();
        itemsTextArea.setBounds(20, 280, 350, 50);
        itemsTextArea.setEditable(false);
        add(itemsTextArea);

        activitiesLabel = new JLabel("Recommended Activities:");
        activitiesLabel.setBounds(20, 340, 200, 20);
        add(activitiesLabel);

        activitiesTextArea = new JTextArea();
        activitiesTextArea.setBounds(20, 360, 350, 50);
        activitiesTextArea.setEditable(false);
        add(activitiesTextArea);
    }

    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == calculateButton) {
            double weight = Double.parseDouble(weightField.getText());
            double height = Double.parseDouble(heightField.getText());
            int age = Integer.parseInt(ageField.getText());

            String gender = maleRadioButton.isSelected() ? "male" : "female";

            double result, bmi;
            String[] items;
            int limit;

            if (gender.equals("male")) {
                result = 66.47 + (13.75 * weight) + (5.003 * height) - (6.755 * age);
                bmi = (weight / Math.pow((height * 0.01), 2)) * 10000;
                items = new String[]{"One-pot shrimp in coconut sauce", "Shrimp tacos", "Vegetarian stuffed peppers Mexican style"};
                limit = 2500;
            } else {
                result = 655.1 + (9.56 * weight) + (1.85 * height) - (4.67 * age);
                bmi = (weight / Math.pow((height * 0.01), 2)) * 10000;
                items = new String[]{"One-pot shrimp in coconut sauce", "Shrimp tacos", "Vegetarian stuffed peppers Mexican style"};
                limit = 2000;
            }

            resultLabel.setText(String.format("You need %.2f calories per day.", result));
            bmiLabel.setText(String.format("Your BMI is: %.2f", bmi));
            itemsTextArea.setText("According to your status, you need to eat these items to get more calories for your body:\n");
            for (String item : items) {
                itemsTextArea.append(item + "\n");
            }

            activitiesTextArea.setText("\nBased on your BMI, we suggest the following activities:\n");
            if (bmi < 18.5) {
                activitiesTextArea.append("Underweight: Add extras to your dishes for more calories, such as cheese in casseroles or nut butter on whole-grain toast. You also can add dry milk or liquid milk to foods for extra protein and calories. Some examples are mashed potatoes or soups. Try smoothies and shakes.");
            } else if (bmi >= 18.5 && bmi < 24.9) {
                activitiesTextArea.append("Normal weight: Maintain a healthy lifestyle with a mix of cardio and strength exercises.");
            } else if (bmi >= 25 && bmi < 29.9) {
                activitiesTextArea.append("Overweight: Focus on cardiovascular exercises like jogging, swimming, and a calorie-controlled diet.");
            } else {
                activitiesTextArea.append("Obese: eat a healthy, reduced-calorie diet and exercise regularly and yoga for flexibility.");
            }

            continueButton.setEnabled(true);
            calculateButton.setEnabled(false);
        } else if (e.getSource() == continueButton) {
            weightField.setText("");
            heightField.setText("");
            ageField.setText("");
            resultLabel.setText("");
            bmiLabel.setText("");
            itemsTextArea.setText("");
            activitiesTextArea.setText("");
            maleRadioButton.setSelected(false);
            femaleRadioButton.setSelected(false);
            continueButton.setEnabled(false);
            calculateButton.setEnabled(true);
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            CalorieCalculatorGUI calculator = new CalorieCalculatorGUI();
            calculator.setVisible(true);
        });
    }
}
