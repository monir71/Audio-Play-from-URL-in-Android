However, for external resources we need internet permission in Manifest file as under:

uses-permission android:name="android.permission.INTERNET"

See the layout:
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/btnPause"
        android:layout_width="0sp"
        android:layout_height="wrap_content"
        android:text="Pause"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/btnPlay"
        app:layout_constraintWidth_percent="0.33"/>

    <Button
        android:id="@+id/btnPlay"
        android:layout_width="0sp"
        android:layout_height="wrap_content"
        android:text="Play"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toEndOf="@+id/btnPause"
        app:layout_constraintEnd_toStartOf="@+id/btnStop"
        app:layout_constraintWidth_percent="0.33"/>

    <Button
        android:id="@+id/btnStop"
        android:layout_width="0sp"
        android:layout_height="wrap_content"
        android:text="Stop"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toEndOf="@+id/btnPlay"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintWidth_percent="0.33"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```
Main Activity:
```
public class MainActivity extends AppCompatActivity {
    Button btnPause, btnPlay, btnStop;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        btnPause = findViewById(R.id.btnPause);
        btnPlay = findViewById(R.id.btnPlay);
        btnStop = findViewById(R.id.btnStop);

        MediaPlayer mediaPlayer = new MediaPlayer();
        mediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);

        String url = "https://alqurans.com/waz/sayeedi/Allah_er_Neyamot_By_Delwar_Hossain_Sayeedi.mp3";
        Uri uri = Uri.parse(url);

        try {
            mediaPlayer.setDataSource(this, uri);
            mediaPlayer.prepare();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

        btnPause.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mediaPlayer.pause();
            }
        });

        btnPlay.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mediaPlayer.start();
            }
        });

        btnStop.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mediaPlayer.pause();
                mediaPlayer.seekTo(0);
            }
        });

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
    }
}
```
