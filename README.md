import javax.sound.sampled.*;
import java.io.File;
import java.io.IOException;
import java.util.Scanner;

public class MusicPlayer {

    private Clip audioClip;
    private AudioInputStream audioStream;
    private String filePath;

    // Load audio file
    public void load(String filePath) throws UnsupportedAudioFileException, IOException, LineUnavailableException {
        this.filePath = filePath;
        File audioFile = new File(filePath);
        audioStream = AudioSystem.getAudioInputStream(audioFile);
        audioClip = AudioSystem.getClip();
        audioClip.open(audioStream);
        System.out.println("Loaded: " + filePath);
    }

    // Play audio
    public void play() {
        if (audioClip != null) {
            audioClip.start();
            System.out.println("Playing...");
        }
    }

    // Pause audio
    public void pause() {
        if (audioClip != null && audioClip.isRunning()) {

            audioClip.stop();
            System.out.println("Paused");
        }
    }

    // Resume audio
    public void resume() {
        if (audioClip != null && !audioClip.isRunning()) {
            audioClip.start();
            System.out.println("Resumed");
        }
    }

    // Stop audio
    public void stop() {
        if (audioClip != null) {
            audioClip.stop();
            audioClip.close();
            System.out.println("Stopped");
        }
    }

    public static void main(String[] args) {
        MusicPlayer player = new MusicPlayer();
        Scanner scanner = new Scanner(System.in);

        try {
            System.out.print("Enter the path of the .wav file: ");
            String filePath = scanner.nextLine();
            player.load(filePath);

            boolean isRunning = true;
            boolean norunning= false;
            while (isRunning) {
                System.out.println("\nCommands: play, pause, resume, stop, exit");
                System.out.print("Enter command: ");
                String command = scanner.nextLine().toLowerCase();

                switch (command) {
                    case "play":
                        player.play();
                        break;
                    case "pause":
                        player.pause();
                        break;
                    case "resume":
                        player.resume();
                        break;
                    case "stop":
                        player.stop();
                        break;
                    case "exit":
                        player.stop();
                        isRunning = false;
                        System.out.println("Exiting...");
                        break;
                    default:
                        System.out.println("Invalid command!");
                }
            }
        } catch (Exception e) {
            System.out.println("An error occurred: " + e.getMessage());
            e.printStackTrace();
        } finally {
            scanner.close();
        }
    }
}

// This player only supports .wav files due to limitations of the javax.sound.sampled package.
// now i learn to make a view for frontend
