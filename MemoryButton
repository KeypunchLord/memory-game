import javafx.scene.control.Button;
import javafx.scene.image.ImageView;

public class MemoryButton extends Button {
        private int buttonId;
        private boolean buttonVisible;
        private ImageView back;
        private ImageView front;

        public MemoryButton(int id) {
            super();
            this.buttonId = id;
            buttonVisible = false;
            //this.setText(String.valueOf(" "));
            back = new ImageView("file:./pics/back.jpg");
            front = new ImageView("file:./pics/" + (buttonId + 1) + ".jpg");
            this.setGraphic(back);
        }

        public int getButtonId() {
            return buttonId;
        }

    public void flip(){
        this.setGraphic(buttonVisible ? back : front);
        //this.setText(buttonVisible ? " " : String.valueOf(buttonId));
        buttonVisible = !buttonVisible;
    }

    public boolean isButtonVisible() {
        return buttonVisible;
    }

    public void setButtonVisible(boolean buttonVisible) {
        this.buttonVisible = buttonVisible;
    }
