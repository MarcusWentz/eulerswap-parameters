# Data Visualization 

To view phase_space we use ffmpeg to construct mp4 videos:

```
for %i in (*.jpg) do ffmpeg -i "%i" -vf scale=1920:-2  "%~ni_resized.jpg"
```

After resizing the images

```
ffmpeg -framerate 12 -i %d.jpg -c:v libx264 -r 30 -vf scale=1920:-2 -pix_fmt yuv420p output.mp4
```
