clear all;
close all;
clc;

image = imread('C:\Users\Limbo\Downloads\rugged_hunter_in_vintage_g-0.jpg');

if size(image, 3) == 3
    gray_image = rgb2gray(image);
else
    gray_image = image;
end

gaussian_filter = fspecial('gaussian', 5, 1.4);
blurred_image = imfilter(double(gray_image), gaussian_filter, 'replicate');

Gx = [-1, 0, 1; 
      -2, 0, 2; 
      -1, 0, 1];

Gy = [-1, -2, -1; 
       0,  0,  0; 
       1,  2,  1];

Sx = imfilter(blurred_image, double(Gx), 'replicate');
Sy = imfilter(blurred_image, double(Gy), 'replicate');

gradient_magnitude = sqrt(Sx.^2 + Sy.^2);
gradient_magnitude = uint8(255 * gradient_magnitude / max(gradient_magnitude(:)));

Sx_display = uint8(255 * (Sx - min(Sx(:))) / (max(Sx(:)) - min(Sx(:))));
Sy_display = uint8(255 * (Sy - min(Sy(:))) / (max(Sy(:)) - min(Sy(:))));

figure('Name', '1. Original Image', 'NumberTitle', 'off');
set(gcf, 'Position', [100, 100, 600, 600]);
imshow(gray_image);
title('Original Image', 'FontSize', 14, 'FontWeight', 'bold');
axis off;

figure('Name', '2. Sobel X', 'NumberTitle', 'off');
set(gcf, 'Position', [750, 100, 600, 600]);
imshow(Sx_display);
title('Sobel X (Horizontal)', 'FontSize', 14, 'FontWeight', 'bold');
axis off;

figure('Name', '3. Sobel Y', 'NumberTitle', 'off');
set(gcf, 'Position', [1400, 100, 600, 600]);
imshow(Sy_display);
title('Sobel Y (Vertical)', 'FontSize', 14, 'FontWeight', 'bold');
axis off;

figure('Name', '4. Sobel Magnitude', 'NumberTitle', 'off');
set(gcf, 'Position', [100, 750, 600, 600]);
imshow(gradient_magnitude);
title('Final Result (Magnitude)', 'FontSize', 14, 'FontWeight', 'bold');
axis off;

figure('Name', 'Sobel Edge Detection - All Together', 'NumberTitle', 'off');
set(gcf, 'Position', [750, 750, 1200, 600]);

subplot(2, 2, 1);
imshow(gray_image);
title('Original Image', 'FontSize', 12, 'FontWeight', 'bold');
axis off;

subplot(2, 2, 2);
imshow(Sx_display);
title('Sobel X (Horizontal)', 'FontSize', 12, 'FontWeight', 'bold');
axis off;

subplot(2, 2, 3);
imshow(Sy_display);
title('Sobel Y (Vertical)', 'FontSize', 12, 'FontWeight', 'bold');
axis off;

subplot(2, 2, 4);
imshow(gradient_magnitude);
title('Final Result', 'FontSize', 12, 'FontWeight', 'bold');
axis off;

sgtitle('Sobel Edge Detection', 'FontSize', 14, 'FontWeight', 'bold');

fprintf('Sobel Edge Detection completed successfully!\n');
fprintf('Image size: %d x %d\n', size(gray_image, 1), size(gray_image, 2));
