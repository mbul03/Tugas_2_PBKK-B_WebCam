# PBKK_TugasKamera_5025201122

| | |
| :--- | :---: |
| Kelas | Pemrograman Berbasis Kerangka Kerja B |
| Dosen Pengampu | Fajar Baskoro, S.Kom., M.T. |
| Nama | Marsyavero Charisyah Putra |
| NRP | 5025201122 |

## Deskripsi Tugas
Membuat aplikasi kamera dengan beberapa fitur, yaitu:

- Memilih kamera yang ingin digunakan
- Menangkap hasil kamera
- Menyimpan hasil kamera

Tampilan program yang akan dibuat sebagai berikut:

![image](https://user-images.githubusercontent.com/97737970/223656377-9f20e471-9253-4fa7-83db-e26131552b12.png)

## Jawaban

### 1. Membuat desain dari program

!<a href="https://ibb.co/xJYV9zX"><img src="https://i.ibb.co/5BkqpsG/SS-webcam1.png" alt="SS-webcam1" border="0" /></a>

### 2. Memasukkan kode program

```
using System;
using System.Drawing.Imaging;
using System.Windows.Forms;
using AForge.Video;
using AForge.Video.DirectShow;

namespace Aplikasi_Kamera_PBKK_B
{
    public partial class Form1 : Form
    {
        private FilterInfoCollection captureDevice;
        private VideoCaptureDevice videoSource;

        public Form1()
        {
            InitializeComponent();
        }

        private void flowLayoutPanel1_Paint(object sender, PaintEventArgs e)
        {
            captureDevice = new FilterInfoCollection(FilterCategory.VideoInputDevice);
        }

        private void tableLayoutPanel1_Paint(object sender, PaintEventArgs e)
        {

        }

        private void Form1_Load(object sender, EventArgs e)
        {
            captureDevice = new FilterInfoCollection(FilterCategory.VideoInputDevice);
            foreach (FilterInfo deviceList in captureDevice)
            {
                comboBox1.Items.Add(deviceList.Name);
            }
            comboBox1.SelectedIndex = 0;
            videoSource = new VideoCaptureDevice();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            if (videoSource.IsRunning)
            {
                videoSource.SignalToStop();
                videoSource.WaitForStop();
                pictureBox1.Image = null;
                pictureBox1.Invalidate();
            }
            videoSource = new VideoCaptureDevice(captureDevice[comboBox1.SelectedIndex].MonikerString);
            videoSource.NewFrame += new NewFrameEventHandler(VideoSource_NewFrame);
            videoSource.Start();
        }

        private void VideoSource_NewFrame(object Sender, NewFrameEventArgs eventArgs)
        {
            pictureBox1.Image = (Bitmap)eventArgs.Frame.Clone();
        }

        private void button2_Click(object sender, EventArgs e)
        {
            pictureBox2.Image = (Bitmap)pictureBox1.Image.Clone();
        }

        private void button3_Click(object sender, EventArgs e)
        {
            SaveFileDialog saveFileDialog = new SaveFileDialog();
            saveFileDialog.Title = "Save Image As";
            saveFileDialog.Filter = "Image files (*.jpg, *.png) | *.jpg *.png";
            ImageFormat imageFormat = ImageFormat.Png;

            if (saveFileDialog.ShowDialog() == DialogResult.OK)
            {
                string ext = System.IO.Path.GetExtension(saveFileDialog.FileName);
                switch (ext)
                {
                    case ".jpg":
                        imageFormat = ImageFormat.Jpeg;
                        break;
                    case ".png":
                        imageFormat = ImageFormat.Png;
                        break;
                }
                pictureBox2.Image.Save(saveFileDialog.FileName, imageFormat);
            }
        }

        private void button4_Click(object sender, EventArgs e)
        {
            if (videoSource.IsRunning)
            {
                videoSource.SignalToStop();
                videoSource.WaitForStop();
                pictureBox1.Image = null;
                pictureBox1.Invalidate();
                pictureBox2.Image = null;
                pictureBox2.Invalidate();

            }
            Application.Exit(null);
        }
    }
}
```

> Menangkap komponen kamera pada perangkat

```
captureDevice = new FilterInfoCollection(FilterCategory.VideoInputDevice);
foreach (FilterInfo deviceList in captureDevice)
{
    comboBox1.Items.Add(deviceList.Name);
}
comboBox1.SelectedIndex = 0;
videoSource = new VideoCaptureDevice();
```


> Button start, untuk mengaktifkan kamera

```
        private void button1_Click(object sender, EventArgs e)
        {
            if (videoSource.IsRunning)
            {
                videoSource.SignalToStop();
                videoSource.WaitForStop();
                pictureBox1.Image = null;
                pictureBox1.Invalidate();
            }
            videoSource = new VideoCaptureDevice(captureDevice[comboBox1.SelectedIndex].MonikerString);
            videoSource.NewFrame += new NewFrameEventHandler(VideoSource_NewFrame);
            videoSource.Start();
        }
        
        private void VideoSource_NewFrame(object Sender, NewFrameEventArgs eventArgs)
        {
            pictureBox1.Image = (Bitmap)eventArgs.Frame.Clone();
        }
```

> Button capture, menangkap hasil kamera

```
        private void button2_Click(object sender, EventArgs e)
        {
            pictureBox2.Image = (Bitmap)pictureBox1.Image.Clone();
        }
```

> Button save image, menyimpan hasil tangkap kamera dalam bentuk jpg atau png

```
        private void button3_Click(object sender, EventArgs e)
        {
            SaveFileDialog saveFileDialog = new SaveFileDialog();
            saveFileDialog.Title = "Save Image As";
            saveFileDialog.Filter = "Image files (*.jpg, *.png) | *.jpg *.png";
            ImageFormat imageFormat = ImageFormat.Png;

            if (saveFileDialog.ShowDialog() == DialogResult.OK)
            {
                string ext = System.IO.Path.GetExtension(saveFileDialog.FileName);
                switch (ext)
                {
                    case ".jpg":
                        imageFormat = ImageFormat.Jpeg;
                        break;
                    case ".png":
                        imageFormat = ImageFormat.Png;
                        break;
                }
                pictureBox2.Image.Save(saveFileDialog.FileName, imageFormat);
            }
        }
```

> Button exit, mematikan kamera dan mengeluarkan program

```
        private void button4_Click(object sender, EventArgs e)
        {
            if (videoSource.IsRunning)
            {
                videoSource.SignalToStop();
                videoSource.WaitForStop();
                pictureBox1.Image = null;
                pictureBox1.Invalidate();
                pictureBox2.Image = null;
                pictureBox2.Invalidate();

            }
            Application.Exit(null);
        }
```

## Hasil

> Memilih kamera yang ingin digunakan

![image](https://user-images.githubusercontent.com/97737970/223658931-9ca761d8-626e-40b5-af54-630fd5b061a4.png)

> Testing kamera

!<a href="https://ibb.co/9YMs4HJ"><img src="https://i.ibb.co/ZTZWgz3/SS-webcam-2.png" alt="SS-webcam-2" border="0"></a>


## Referensi
https://www.youtube.com/watch?v=HUiV10g1VLU
