# mcclick
import java.awt.Robot;
import java.awt.event.InputEvent;
import java.awt.event.KeyEvent;
import java.util.concurrent.atomic.AtomicBoolean;

public class AutoClicker {
    private static AtomicBoolean running = new AtomicBoolean(false);

    public static void main(String[] args) {
        try {
            Robot robot = new Robot();

            // Listener para capturar la tecla R
            Thread keyListener = new Thread(() -> {
                try {
                    while (true) {
                        if (java.awt.Toolkit.getDefaultToolkit().getLockingKeyState(KeyEvent.VK_R)) {
                            running.set(!running.get()); // Cambia el estado cada vez que presionas R
                            Thread.sleep(200); // Previene múltiples cambios al presionar una vez
                        }
                        Thread.sleep(50); // Delay para evitar uso intensivo de CPU
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            });

            // Autoclicker en ejecución
            Thread autoClicker = new Thread(() -> {
                try {
                    while (true) {
                        if (running.get()) {
                            robot.mousePress(InputEvent.BUTTON1_DOWN_MASK);
                            Thread.sleep(100); // Tiempo entre clics
                            robot.mouseRelease(InputEvent.BUTTON1_DOWN_MASK);
                        }
                        Thread.sleep(10);
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            });

            keyListener.start();
            autoClicker.start();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
