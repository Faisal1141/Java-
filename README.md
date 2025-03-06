package BirdAnimation;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class BirdAnimation {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(AnimationGUI::new);
    }
}



class AnimationGUI {
    private JFrame frame;
    private JPanel controlPanel;
    private JComboBox<String> objectSelector;
    private JButton startButton;
    private JSlider speedSlider;
    private AnimationPanel animationPanel;
    private Timer animationTimer;
    private int speed = 5;
    private ImageIcon[] birdSprites = new ImageIcon[4];
    private ImageIcon[] planeSprites = new ImageIcon[4];
    private ImageIcon[] currentSprites;
    private int spriteIndex = 0;

    public AnimationGUI() {
        
        
        
    
        birdSprites[0] = new ImageIcon("bird_a.png");
        birdSprites[1] = new ImageIcon("bird_b.png");
        birdSprites[2] = new ImageIcon("bird_c.png");
        birdSprites[3] = new ImageIcon("bird_d.png");

        
        
        planeSprites[0] = new ImageIcon("plane_a.png");
        planeSprites[1] = new ImageIcon("plane_b.png");
        planeSprites[2] = new ImageIcon("plane_c.png");
        planeSprites[3] = new ImageIcon("plane_d.png");

        currentSprites = birdSprites;

        frame = new JFrame("Animation Program");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new BorderLayout());

        controlPanel = new JPanel();
        objectSelector = new JComboBox<>(new String[]{"Bird", "Plane"});
        startButton = new JButton("Start Animation");
        speedSlider = new JSlider(1, 20, 5);
        controlPanel.add(new JLabel("Select Object:"));
        controlPanel.add(objectSelector);
        controlPanel.add(startButton);
        controlPanel.add(new JLabel("Speed:"));
        controlPanel.add(speedSlider);

        
        
        animationPanel = new AnimationPanel();
        frame.add(controlPanel, BorderLayout.NORTH);
        frame.add(animationPanel, BorderLayout.CENTER);

        startButton.addActionListener(e -> startAnimation());

        objectSelector.addActionListener(e -> {
            String selected = (String) objectSelector.getSelectedItem();
            if ("Bird".equals(selected)) {
                currentSprites = birdSprites;
            } else if ("Plane".equals(selected)) {
                currentSprites = planeSprites;
            }
        });

        speedSlider.addChangeListener(e -> {
            speed = speedSlider.getValue();
            if (animationTimer != null) {
                animationTimer.setDelay(100 / speed);
            }
        });

        frame.setSize(600, 400);
        frame.setVisible(true);
    }

    private void startAnimation() {
        animationTimer = new Timer(100 / speed, e -> {
            spriteIndex = (spriteIndex + 1) % currentSprites.length;
            animationPanel.repaint();
        });
        animationTimer.start();
    }

    class AnimationPanel extends JPanel {
        
        protected void paintComponent(Graphics g) {
            super.paintComponent(g);
            Image objectImage = currentSprites[spriteIndex].getImage();
            g.drawImage(objectImage, 0, 0, getWidth(), getHeight(), this);
        }
    }
}
