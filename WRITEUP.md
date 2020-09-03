# **Finding Lane Lines on the Road** 

### Reflection

### 1. Pipeline

The pipeline is defined by this function:
```python
def detect_lane_lines(image):
    return run_pipeline(image, [
        apply_grayscale,
        apply_gaussian_blur,
        apply_canny_edge_detection,
        apply_region_of_interest,
        apply_extrapolated_hough_lines
    ])
```

- Convert to grayscale to be able to apply Canny Edge Detection later
- Apply Gaussian Blur to reduce noise
- Apply Canny Edge Detection to get only the edges in the image
- Remove/Ignore everything that is outside of a defined region of interest
- Apply extrapolated Hough lines function
  - Apply Hough lines to get the lines of the image
  - Calculate the slope of those lines and split them in two groups: positive and negative
  - Get the ~~average~~ median positive slope and the ~~average~~ median negative slope
  - Get the two lines that have the closest slope to the average ones
  - Extrapolate those lines by calculating the X for a given hard-coded Y
  - Draw these two lines over the original image


### 2. Shortcomings with the current pipeline

- Lines are not detected if there is little contrast (this can be too dark or too bright scenarios)
- If no lines are found, at some point there is a division by zero that breaks the pipeline
- The drawn lines jitter a bit
- The slope of the lines sometimes is not really good
- It does not perform well with curves
- I don't think it'll work with steep roads


### 3. Suggest possible improvements to your pipeline

- I could remove outliars of the list of slopes
- I could try to reduce the contrast problem by transforming the color space to HSV and finding yellow/white colors
- The jittering could be reduced by averaging with the previous frame
