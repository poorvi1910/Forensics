## LITCTF: Kuwahara
Reference: https://liminova.github.io/blog/litctf-2024-kuwahara

Brief explanantion of the writeup:

The challenge provided a noisy image and an encoded image. The goal was to extract the flag from the encoded image.The encoded image was created by applying the Kuwahara filter to the noisy image, which involved hiding the message using a base-3 encoding. This filter works by dividing the image into regions, calculating the average color and standard deviation in each region, and then replacing each pixel with the color from the region with the lowest variation.
By determining which region's average color was used in each part of the encoded image, they could reverse the base-3 encoding to extract the original message.
The solver initially guessed the window size (the size of the regions) incorrectly, leading to an unsuccessful decoding attempt. And even after correcting the window size, the decoded message had errors. These were due to small inaccuracies in the color values, likely caused by floating-point errors in the calculations.
To overcome these issues, the solver modified their approach by considering all possible regions (not just the one with the lowest variation) when trying to decode each character.
When the decoded values were slightly off (by a value of 1), the solver manually adjusted them to the correct base-3 values.

This was a very scripting intensive challenge and i got to know about the filter, reversing procedure and error correction. 

Summary of script:

Libraries:
numpy: A library used for numerical operations, especially working with arrays.
PIL (Python Imaging Library): Used for opening, manipulating, and saving images. Here, it's used to load images as arrays.
tqdm: A library to show progress bars for loops. This is helpful for visual feedback when processing large datasets or images.
matplotlib: A library used for plotting graphs and visualizing data. Here, it's imported but not used in the provided code snippet.
Image.open(): Opens an image file and converts it into an array using np.array(). This makes it easier to manipulate the image.

```
window_size = 5  # Size of the Kuwahara filter window
step_size = 1    # Step size for moving the window
```
window_size: This is the size of the region used in the Kuwahara filter. The filter calculates statistics within this region to determine how to encode/decode the image.
step_size: This determines how much the window moves across the image. A step size of 1 means the window moves one pixel at a time.

```
def extract_mean_std(image, x, y, window_size):
    half_window = window_size // 2
    windows = [
        image[max(0, x-half_window):x+1, max(0, y-half_window):y+1],
        image[max(0, x-half_window):x+1, y:y+half_window+1],
        image[x:x+half_window+1, max(0, y-half_window):y+1],
        image[x:x+half_window+1, y:y+half_window+1]
    ]
    mean_std = [(np.mean(w), np.std(w)) for w in windows]
    return mean_std
```
half_window: Half of the window_size. This is used to define the regions around the pixel.
windows: The four regions around the pixel (x, y) in the image. Each region is a subset of the image array.
np.mean(w): Calculates the average color value in each region.
np.std(w): Calculates the standard deviation (variation) in each region.
mean_std: A list of tuples containing the mean and standard deviation for each region.
This function returns the mean and standard deviation for each of the four regions around a given pixel.

```
def decode_pixel(encoded_pixel, mean_std_noisy, threshold=0.1):
    differences = [abs(mean - encoded_pixel) for mean, _ in mean_std_noisy]
    min_diff_index = np.argmin(differences)
    if differences[min_diff_index] > threshold:
        return None
    return min_diff_index
```
encoded_pixel: The value of the pixel in the encoded image.
mean_std_noisy: The list of mean and standard deviation tuples from the noisy image.
differences: Calculates the absolute difference between the encoded pixel value and the mean of each region.
np.argmin(differences): Finds the index of the region with the smallest difference (i.e., the best match).
threshold: A value to check if the smallest difference is too large. If it is, the function returns None, indicating that decoding isn't reliable.
min_diff_index: The index of the best matching region, which corresponds to the base-3 digit used in encoding.
This function attempts to reverse the encoding by finding which region in the noisy image the encoded pixel came from.

```
decoded_message = []
for i in tqdm(range(half_window, encoded_img.shape[0] - half_window)):
    row_message = []
    for j in range(half_window, encoded_img.shape[1] - half_window):
        mean_std_noisy = extract_mean_std(noisy_img, i, j, window_size)
        decoded_pixel = decode_pixel(encoded_img[i, j], mean_std_noisy)
        if decoded_pixel is not None:
            row_message.append(str(decoded_pixel))
        else:
            row_message.append("?")
    decoded_message.append(row_message)
```
decoded_message: A list that will store the decoded message, row by row.
tqdm(range(...)): A loop over each pixel in the encoded image, with tqdm showing a progress bar.
extract_mean_std(): Called for each pixel to get the mean and standard deviation of the four regions around it.
decode_pixel(): Uses the means and standard deviations to decode the pixel's value.
row_message.append(): Adds the decoded value (or a "?" if decoding failed) to the current row.
This loop goes through every pixel in the encoded image (excluding the borders) and decodes it, building up the message row by row.
```
decoded_message_str = '\n'.join([''.join(row) for row in decoded_message])
print(decoded_message_str)
```
decoded_message_str: Converts the list of decoded rows into a single string, with each row separated by a newline (\n).
print(decoded_message_str): Prints the final decoded message to the console.

