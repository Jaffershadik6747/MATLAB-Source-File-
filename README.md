#  % Parameters
gridSize = 200; % Size of the grid (higher = finer resolution)
craterDensity = 0.02; % Density of craters (higher = more craters)
maxCraterDepth = -2; % Maximum depth of craters (negative for depressions)
volcanicHeight = 5; % Height of volcanic features
smoothness = 2; % Smoothness of the terrain (higher = smoother)
 % Generate base terrain (random noise)
terrain = randn(gridSize); % Random noise
terrain = imgaussfilt(terrain, smoothness); % Smooth the terrain
 % Add craters (circular depressions)
numCraters = round(craterDensity * gridSize^2);
for i = 1:numCraters
    % Random crater center and radius
    centerX = randi([1, gridSize]);
    centerY = randi([1, gridSize]);
    radius = randi([5, 20]); % Random radius
     % Create a crater mask
    [X, Y] = meshgrid(1:gridSize, 1:gridSize);
    craterMask = ((X - centerX).^2 + (Y - centerY).^2) <= radius^2;
     % Add the crater to the terrain
    terrain(craterMask) = terrain(craterMask) + maxCraterDepth * (1 - ((X(craterMask) - centerX).^2 + (Y(craterMask) - centerY).^2) / radius^2);
end
 
% Add volcanic features (random peaks)
numVolcanoes = 5; % Number of volcanic peaks
for i = 1:numVolcanoes
    % Random volcano center
    centerX = randi([1, gridSize]);
    centerY = randi([1, gridSize]);
    
    % Create a volcano mask
    [X, Y] = meshgrid(1:gridSize, 1:gridSize);
    volcanoMask = ((X - centerX).^2 + (Y - centerY).^2) <= 100; % Broad base
    
    % Add the volcano to the terrain
    terrain(volcanoMask) = terrain(volcanoMask) + volcanicHeight * exp(-((X(volcanoMask) - centerX).^2 + (Y(volcanoMask) - centerY).^2) / 50);
end
 % Plot the terrain
figure;
mesh(terrain);
colormap('turbo'); % Use a colormap to represent elevation
title('Noachian Era Terrain on Ancient Mars');
xlabel('X (km)');
ylabel('Y (km)');
zlabel('Elevation (km)');
colorbar; % Add a colorbar to show elevation
axis tight;
view(3); % 3D view
grid on;
 % Observations
disp('Observations from the graph:');
disp('1. The terrain is heavily cratered, reflecting the intense bombardment during the Noachian era.');
disp('2. Volcanic features (peaks) are visible, indicating volcanic activity.');
disp('3. The surface is uneven, with both depressions (craters) and elevated regions (volcanoes).');
disp('4. The presence of liquid water could be inferred from the smoother regions between craters.');
