# FIJI_DefineRandomSpots
This FIJI (ImageJ) macro generates a user-specified number of random point ROIs within a selected region of interest (ROI) and adds them to the ROI Manager.

# Features
Prompts user for the number of random points to generate
Ensures all generated points fall within the selected ROI shape (not just the bounding box)
Automatically adds valid point ROIs to the ROI Manager

# Requirements
FIJI (ImageJ) with macro support
An ROI must be selected before running the macro

# Usage
Open your image in FIJI.
Draw or load a region of interest (ROI).
Run the DefineRandomSpots.ijm macro.
Enter the desired number of random points in the prompt.
The macro will generate and add the points to the ROI Manager.

# Example
If you select a circular ROI and request 50 random points, the macro will place 50 point ROIs randomly distributed within the circle.

# Define Random ROIs Macro Script

```javascript
// DefineRandomSpots
// This ImageJ macro generates a user-specified number of random point ROIs within a selected
// ROI and adds them all to the ROI manager

#@int(label = "Number of random points to generate:") numPoints  // Prompt the user to enter how many random points to generate

roiManager("Add");  // Add the currently selected ROI (assumed to be the large area) to the ROI Manager

Roi.getBounds(x, y, w, h);  // Get the bounding box of the ROI: x, y (top-left), w (width), h (height)

count = 0;  // Initialize counter for how many points have been successfully generated

while (count < numPoints) {  // Loop until the desired number of valid points is generated
    roiManager("select", 0);  // Select the first ROI in the ROI Manager (the big region)

    x1 = random() * w + x;  // Generate a random x coordinate within the bounding box
    y1 = random() * h + y;  // Generate a random y coordinate within the bounding box

    if (selectionContains(x1, y1) == true ) {  // Check if the generated point lies within the ROI shape
        makePoint(x1, y1);  // Create a point at the random (x1, y1) position
        roiManager("Add");  // Add the point ROI to the ROI Manager
        count++;  // Increment the counter only if the point was inside the ROI and added
    }
}


