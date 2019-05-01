import javafx.application.Application;
import javafx.application.Platform;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;
import javafx.scene.text.Text;
import javafx.stage.Modality;
import javafx.stage.Stage;

import java.util.Random;


public class Memory extends Application {
    private Label player1 = new Label();
    private Label player2 = new Label();
    private Label status = new Label();
    private MemoryButton[] memoryButtons;
    private MemoryButton[] flippedButtons = new MemoryButton[2];
    private int openButtons = 0;
    private int actPlayer;
    private int scorePlayerOne;
    private int scorePlayerTwo;
    private int size = 4;


    private Scene buildGui() {
        memoryButtons = new MemoryButton[size*size];
        BorderPane root = new BorderPane();
        Scene scene = new Scene(root);

        root.getStylesheets().add(getClass().getResource("MemoryDesign.css").toExternalForm());


        MenuBar menuBar = new MenuBar();
        Menu fileMenu = new Menu("Options");
        Menu newGameItem = new Menu("New Game");
        MenuItem newGameEasy = new MenuItem("Easy");
        MenuItem newGameClassic = new MenuItem("Classic");
        MenuItem newGameDifficult = new MenuItem("Difficult");
        MenuItem exitGameItem = new MenuItem("Exit");

        fileMenu.getItems().addAll(newGameItem, exitGameItem);
        newGameItem.getItems().addAll(newGameEasy,newGameClassic,newGameDifficult);
        menuBar.getMenus().addAll(fileMenu);
        exitGameItem.setOnAction(event -> Platform.exit());
        newGameEasy.setOnAction(event -> startNewGame(4));
        newGameClassic.setOnAction(event -> startNewGame(6));
        newGameDifficult.setOnAction(event -> startNewGame(8));


        VBox top = new VBox();

        // Anzeige Spielerinfo oben
        HBox punkte = new HBox(350, player1, player2);

        top.getChildren().addAll(menuBar, punkte);
        root.setTop(top);

        for (int i = 0; i < size*size; i++) {
            memoryButtons[i] = new MemoryButton(i /2);
        }
        shuffleMemoryButtons();

        // Anzeige Memorykarten MITTE
        GridPane arena = new GridPane();
        for (int y = 0; y < size; y++) {
            for (int x = 0; x < size; x++) {
                MemoryButton b = memoryButtons[y * size + x];

                b.setPrefSize(100,100);
                b.setOnAction(e -> checkMemoryButtons(b));
                arena.add(b, x, y);
            }
        }
        root.setCenter(arena);

        startNewGame(4);

        // Anzeige Spielstatus unten
        root.setBottom(status);

        return scene;
    }

    private void checkMemoryButtons(MemoryButton b) {
        if (openButtons == 0){ // noch kein MemoryButton umgedreht
            flippedButtons[0] = b;
            b.flip();
            openButtons++;
        }
        else if(openButtons == 1){ // bereits ein Button umgedreht -> das ist der 2.
            if (!b.isButtonVisible()) {
                flippedButtons[1] = b;
                b.flip();
                openButtons++;
                checkEqual();
            }
        }
        else if(openButtons == 2) { // 2 sind schon offen  -> das ist der 3.
            if (!flippedButtons[0].isDisabled()){
                flippedButtons[0].flip();
                flippedButtons[1].flip();
            }
            flippedButtons[0] = b;
            b.flip();
            openButtons = 1;
        }
    }


    private void checkEqual() {
        Stage winner = new Stage();
        winner.initModality(Modality.APPLICATION_MODAL);
        VBox winnerVBox = new VBox(20);

        /*Alert alert = new Alert(Alert.AlertType.INFORMATION);
        alert.setTitle("You are a Winner!");
        alert.setHeaderText(null);*/

//buttons verschieden?
        if ((flippedButtons[0].getButtonId() == flippedButtons[1].getButtonId()) && (flippedButtons[0] != flippedButtons[1])){
            flippedButtons[0].setDisable(true);
            flippedButtons[1].setDisable(true);

            //punkte erhöhen & status aktualisieren
            if (actPlayer == 0)
                scorePlayerOne++;
            else
                scorePlayerTwo++;

            player1.setText("Player 1: " + scorePlayerOne);
            player2.setText("Player 2: " + scorePlayerTwo);

            //check win
            if ((scorePlayerOne + scorePlayerTwo)*2 >= memoryButtons.length){
                if (scorePlayerOne == scorePlayerTwo)
                    //alert.setGraphic(new ImageView("file:./pics/draw.jpg"));
                    winnerVBox.getChildren().add(new ImageView("file:./pics/draw.jpg"));
                else if (scorePlayerOne > scorePlayerTwo)
                    //alert.setGraphic(new ImageView("file:./pics/playerOneWins.jpg"));
                    winnerVBox.getChildren().add(new ImageView("file:./pics/playerOneWins.jpg"));
                else
                    //alert.setGraphic(new ImageView("file:./pics/playerTwoWins.jpg"));
                    winnerVBox.getChildren().add(new ImageView("file:./pics/playerTwoWins.jpg"));


                Scene dialogScene = new Scene(winnerVBox, 300, 200);
                winner.setScene(dialogScene);
                winner.show();

                //alert.show();
            }
        }
        else{
            //spielerwechsel
            actPlayer = (actPlayer + 1) % 2;
            status.setText("Player " + (actPlayer + 1) + " It's your turn");
        }
    }

    private void shuffleMemoryButtons() {
        Random r = new Random();
        int j;
        MemoryButton help;

        for (int i = 0; i < memoryButtons.length; i++) {
            j = r.nextInt(memoryButtons.length);

            help = memoryButtons[i];
            memoryButtons[i] = memoryButtons[j];
            memoryButtons[j] = help;
        }
    }

    private void startNewGame(int size) {
        size = this.size;
        actPlayer = 0;
        scorePlayerOne = 0;
        scorePlayerTwo = 0;
        openButtons = 0;

        player1.setText("Player 1: " + scorePlayerOne);
        player2.setText("Player 2: " + scorePlayerTwo);
        status.setText("Player " + (actPlayer + 1) + " It's your turn");

        flippedButtons[0] = null;
        flippedButtons[1] = null;

        for (MemoryButton memoryButton : memoryButtons) {
            if (memoryButton.isButtonVisible()) {
                memoryButton.flip();
            }
            memoryButton.setDisable(false);
            memoryButton.setButtonVisible(false);
        }
        shuffleMemoryButtons();
    }


    @Override
    public void start(Stage primaryStage) throws Exception {
        Scene scene = buildGui();

        primaryStage.setScene(scene);
        primaryStage.setTitle("Queen of Memory");
        primaryStage.getIcons().add(new Image("file:./pics/icon.gif"));
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }

}
