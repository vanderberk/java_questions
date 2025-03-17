# Rectangle

**Difficulty**: Easy  
**Tags**: Math, Geometry, Object-Oriented Design

## Question
Implement a `Rectangle` class that represents a rectangle in a 2D plane. Your implementation should include:

1. A constructor that creates a rectangle with a given width, height, and position (x, y coordinates of the bottom-left corner).
2. Methods to:
   - Calculate the area of the rectangle.
   - Calculate the perimeter of the rectangle.
   - Determine if a point is inside the rectangle.
   - Check if two rectangles intersect.
   - Calculate the area of intersection between two rectangles.
   - Translate (move) the rectangle by a given offset.
   - Rotate the rectangle around its center by a given angle.

## Example Input/Output
```
Rectangle rect1 = new Rectangle(10, 5, 0, 0); // width=10, height=5, position=(0,0)
System.out.println(rect1.getArea()); // Returns 50.0
System.out.println(rect1.getPerimeter()); // Returns 30.0

Point p = new Point(3, 2);
System.out.println(rect1.containsPoint(p)); // Returns true

Rectangle rect2 = new Rectangle(8, 4, 5, 2);
System.out.println(rect1.intersects(rect2)); // Returns true
System.out.println(rect1.intersectionArea(rect2)); // Returns 20.0

rect1.translate(2, 3); // Moves the rectangle to position (2, 3)
rect1.rotate(45); // Rotates the rectangle 45 degrees around its center
```

## Solution Algorithm
To implement the `Rectangle` class, we need to:

1. Store the rectangle's width, height, and position (bottom-left corner).
2. Calculate area as width × height.
3. Calculate perimeter as 2 × (width + height).
4. To check if a point is inside the rectangle, verify that the point's coordinates are within the rectangle's bounds.
5. To check if two rectangles intersect, verify that one rectangle is not completely to the left, right, above, or below the other.
6. To calculate the intersection area, find the overlapping rectangle and calculate its area.
7. To translate the rectangle, add the offset to the position coordinates.
8. To rotate the rectangle, use trigonometric functions to rotate the corners around the center.

## Solution Code
```java
public class Point {
    private double x;
    private double y;
    
    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    public double getX() {
        return x;
    }
    
    public double getY() {
        return y;
    }
    
    public void translate(double dx, double dy) {
        this.x += dx;
        this.y += dy;
    }
    
    public void rotate(Point center, double angleDegrees) {
        double angleRadians = Math.toRadians(angleDegrees);
        double cosAngle = Math.cos(angleRadians);
        double sinAngle = Math.sin(angleRadians);
        
        // Translate point to origin
        double translatedX = x - center.x;
        double translatedY = y - center.y;
        
        // Rotate point
        double rotatedX = translatedX * cosAngle - translatedY * sinAngle;
        double rotatedY = translatedX * sinAngle + translatedY * cosAngle;
        
        // Translate point back
        this.x = rotatedX + center.x;
        this.y = rotatedY + center.y;
    }
    
    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}

public class Rectangle {
    private double width;
    private double height;
    private Point bottomLeft;
    private Point bottomRight;
    private Point topLeft;
    private Point topRight;
    private boolean isRotated;
    
    /**
     * Create a rectangle with the given width, height, and position.
     * @param width The width of the rectangle.
     * @param height The height of the rectangle.
     * @param x The x-coordinate of the bottom-left corner.
     * @param y The y-coordinate of the bottom-left corner.
     */
    public Rectangle(double width, double height, double x, double y) {
        this.width = width;
        this.height = height;
        this.bottomLeft = new Point(x, y);
        this.bottomRight = new Point(x + width, y);
        this.topLeft = new Point(x, y + height);
        this.topRight = new Point(x + width, y + height);
        this.isRotated = false;
    }
    
    /**
     * Get the area of the rectangle.
     * @return The area.
     */
    public double getArea() {
        return width * height;
    }
    
    /**
     * Get the perimeter of the rectangle.
     * @return The perimeter.
     */
    public double getPerimeter() {
        return 2 * (width + height);
    }
    
    /**
     * Get the center of the rectangle.
     * @return The center point.
     */
    public Point getCenter() {
        if (!isRotated) {
            return new Point(bottomLeft.getX() + width / 2, bottomLeft.getY() + height / 2);
        } else {
            // For rotated rectangles, calculate the center as the average of the four corners
            double centerX = (bottomLeft.getX() + bottomRight.getX() + topLeft.getX() + topRight.getX()) / 4;
            double centerY = (bottomLeft.getY() + bottomRight.getY() + topLeft.getY() + topRight.getY()) / 4;
            return new Point(centerX, centerY);
        }
    }
    
    /**
     * Check if a point is inside the rectangle.
     * @param p The point to check.
     * @return true if the point is inside the rectangle, false otherwise.
     */
    public boolean containsPoint(Point p) {
        if (!isRotated) {
            // For non-rotated rectangles, simple bounds check
            return p.getX() >= bottomLeft.getX() && p.getX() <= bottomRight.getX() &&
                   p.getY() >= bottomLeft.getY() && p.getY() <= topLeft.getY();
        } else {
            // For rotated rectangles, use the cross product method
            // Check if the point is on the same side of all four edges
            return isPointOnSameSide(bottomLeft, bottomRight, topRight, p) &&
                   isPointOnSameSide(bottomRight, topRight, topLeft, p) &&
                   isPointOnSameSide(topRight, topLeft, bottomLeft, p) &&
                   isPointOnSameSide(topLeft, bottomLeft, bottomRight, p);
        }
    }
    
    /**
     * Helper method to check if a point is on the same side of a line as a reference point.
     */
    private boolean isPointOnSameSide(Point lineStart, Point lineEnd, Point reference, Point point) {
        double crossProduct1 = crossProduct(lineStart, lineEnd, reference);
        double crossProduct2 = crossProduct(lineStart, lineEnd, point);
        return crossProduct1 * crossProduct2 >= 0;
    }
    
    /**
     * Calculate the cross product (p2 - p1) × (p3 - p1).
     */
    private double crossProduct(Point p1, Point p2, Point p3) {
        return (p2.getX() - p1.getX()) * (p3.getY() - p1.getY()) - 
               (p2.getY() - p1.getY()) * (p3.getX() - p1.getX());
    }
    
    /**
     * Check if this rectangle intersects with another rectangle.
     * @param other The other rectangle.
     * @return true if the rectangles intersect, false otherwise.
     */
    public boolean intersects(Rectangle other) {
        if (!isRotated && !other.isRotated) {
            // For non-rotated rectangles, simple bounds check
            return !(bottomRight.getX() < other.bottomLeft.getX() ||
                     bottomLeft.getX() > other.bottomRight.getX() ||
                     topLeft.getY() < other.bottomLeft.getY() ||
                     bottomLeft.getY() > other.topLeft.getY());
        } else {
            // For rotated rectangles, check if any corner of one rectangle is inside the other
            // or if any edge of one rectangle intersects with any edge of the other
            return containsAnyPoint(other) || other.containsAnyPoint(this) || edgesIntersect(other);
        }
    }
    
    /**
     * Check if any corner of the other rectangle is inside this rectangle.
     */
    private boolean containsAnyPoint(Rectangle other) {
        return containsPoint(other.bottomLeft) || containsPoint(other.bottomRight) ||
               containsPoint(other.topLeft) || containsPoint(other.topRight);
    }
    
    /**
     * Check if any edge of this rectangle intersects with any edge of the other rectangle.
     */
    private boolean edgesIntersect(Rectangle other) {
        // Check all possible edge pairs
        return lineSegmentsIntersect(bottomLeft, bottomRight, other.bottomLeft, other.bottomRight) ||
               lineSegmentsIntersect(bottomLeft, bottomRight, other.bottomRight, other.topRight) ||
               lineSegmentsIntersect(bottomLeft, bottomRight, other.topRight, other.topLeft) ||
               lineSegmentsIntersect(bottomLeft, bottomRight, other.topLeft, other.bottomLeft) ||
               lineSegmentsIntersect(bottomRight, topRight, other.bottomLeft, other.bottomRight) ||
               lineSegmentsIntersect(bottomRight, topRight, other.bottomRight, other.topRight) ||
               lineSegmentsIntersect(bottomRight, topRight, other.topRight, other.topLeft) ||
               lineSegmentsIntersect(bottomRight, topRight, other.topLeft, other.bottomLeft) ||
               lineSegmentsIntersect(topRight, topLeft, other.bottomLeft, other.bottomRight) ||
               lineSegmentsIntersect(topRight, topLeft, other.bottomRight, other.topRight) ||
               lineSegmentsIntersect(topRight, topLeft, other.topRight, other.topLeft) ||
               lineSegmentsIntersect(topRight, topLeft, other.topLeft, other.bottomLeft) ||
               lineSegmentsIntersect(topLeft, bottomLeft, other.bottomLeft, other.bottomRight) ||
               lineSegmentsIntersect(topLeft, bottomLeft, other.bottomRight, other.topRight) ||
               lineSegmentsIntersect(topLeft, bottomLeft, other.topRight, other.topLeft) ||
               lineSegmentsIntersect(topLeft, bottomLeft, other.topLeft, other.bottomLeft);
    }
    
    /**
     * Check if two line segments intersect.
     */
    private boolean lineSegmentsIntersect(Point p1, Point p2, Point p3, Point p4) {
        // Calculate the direction of the cross products
        double d1 = crossProduct(p3, p4, p1);
        double d2 = crossProduct(p3, p4, p2);
        double d3 = crossProduct(p1, p2, p3);
        double d4 = crossProduct(p1, p2, p4);
        
        // Check if the line segments intersect
        if (d1 * d2 < 0 && d3 * d4 < 0) {
            return true;
        }
        
        // Check if any endpoint lies on the other line segment
        if (d1 == 0 && isPointOnLineSegment(p3, p4, p1)) return true;
        if (d2 == 0 && isPointOnLineSegment(p3, p4, p2)) return true;
        if (d3 == 0 && isPointOnLineSegment(p1, p2, p3)) return true;
        if (d4 == 0 && isPointOnLineSegment(p1, p2, p4)) return true;
        
        return false;
    }
    
    /**
     * Check if a point is on a line segment.
     */
    private boolean isPointOnLineSegment(Point lineStart, Point lineEnd, Point point) {
        // Check if the point is collinear with the line segment
        if (crossProduct(lineStart, lineEnd, point) != 0) {
            return false;
        }
        
        // Check if the point is within the bounding box of the line segment
        return point.getX() >= Math.min(lineStart.getX(), lineEnd.getX()) &&
               point.getX() <= Math.max(lineStart.getX(), lineEnd.getX()) &&
               point.getY() >= Math.min(lineStart.getY(), lineEnd.getY()) &&
               point.getY() <= Math.max(lineStart.getY(), lineEnd.getY());
    }
    
    /**
     * Calculate the area of intersection between this rectangle and another rectangle.
     * @param other The other rectangle.
     * @return The area of intersection.
     */
    public double intersectionArea(Rectangle other) {
        if (!isRotated && !other.isRotated) {
            // For non-rotated rectangles, calculate the overlapping rectangle
            double x1 = Math.max(bottomLeft.getX(), other.bottomLeft.getX());
            double y1 = Math.max(bottomLeft.getY(), other.bottomLeft.getY());
            double x2 = Math.min(bottomRight.getX(), other.bottomRight.getX());
            double y2 = Math.min(topLeft.getY(), other.topLeft.getY());
            
            // Check if there is an intersection
            if (x2 < x1 || y2 < y1) {
                return 0;
            }
            
            return (x2 - x1) * (y2 - y1);
        } else {
            // For rotated rectangles, this is a complex problem
            // A simple approximation is to use a Monte Carlo method
            throw new UnsupportedOperationException("Intersection area for rotated rectangles is not implemented");
        }
    }
    
    /**
     * Translate (move) the rectangle by the given offset.
     * @param dx The x offset.
     * @param dy The y offset.
     */
    public void translate(double dx, double dy) {
        bottomLeft.translate(dx, dy);
        bottomRight.translate(dx, dy);
        topLeft.translate(dx, dy);
        topRight.translate(dx, dy);
    }
    
    /**
     * Rotate the rectangle around its center by the given angle in degrees.
     * @param angleDegrees The angle in degrees.
     */
    public void rotate(double angleDegrees) {
        Point center = getCenter();
        
        bottomLeft.rotate(center, angleDegrees);
        bottomRight.rotate(center, angleDegrees);
        topLeft.rotate(center, angleDegrees);
        topRight.rotate(center, angleDegrees);
        
        isRotated = true;
    }
    
    @Override
    public String toString() {
        return "Rectangle[width=" + width + ", height=" + height + 
               ", bottomLeft=" + bottomLeft + ", bottomRight=" + bottomRight + 
               ", topLeft=" + topLeft + ", topRight=" + topRight + "]";
    }
    
    public static void main(String[] args) {
        // Create a rectangle
        Rectangle rect1 = new Rectangle(10, 5, 0, 0);
        System.out.println("Rectangle 1: " + rect1);
        System.out.println("Area: " + rect1.getArea());
        System.out.println("Perimeter: " + rect1.getPerimeter());
        
        // Check if a point is inside the rectangle
        Point p = new Point(3, 2);
        System.out.println("Is point " + p + " inside rectangle 1? " + rect1.containsPoint(p));
        
        // Create another rectangle
        Rectangle rect2 = new Rectangle(8, 4, 5, 2);
        System.out.println("Rectangle 2: " + rect2);
        
        // Check if the rectangles intersect
        System.out.println("Do the rectangles intersect? " + rect1.intersects(rect2));
        
        // Calculate the area of intersection
        System.out.println("Area of intersection: " + rect1.intersectionArea(rect2));
        
        // Translate the first rectangle
        rect1.translate(2, 3);
        System.out.println("Rectangle 1 after translation: " + rect1);
        
        // Rotate the first rectangle
        rect1.rotate(45);
        System.out.println("Rectangle 1 after rotation: " + rect1);
        
        // Check if the point is still inside the rectangle
        System.out.println("Is point " + p + " inside rectangle 1 after rotation? " + rect1.containsPoint(p));
    }
}
```

```java
// Alternative implementation with additional features
public class RectangleUtils {
    /**
     * Calculate the union area of two rectangles.
     * @param rect1 The first rectangle.
     * @param rect2 The second rectangle.
     * @return The union area.
     */
    public static double unionArea(Rectangle rect1, Rectangle rect2) {
        return rect1.getArea() + rect2.getArea() - rect1.intersectionArea(rect2);
    }
    
    /**
     * Calculate the minimum bounding rectangle for two rectangles.
     * @param rect1 The first rectangle.
     * @param rect2 The second rectangle.
     * @return The minimum bounding rectangle.
     */
    public static Rectangle boundingRectangle(Rectangle rect1, Rectangle rect2) {
        // Find the minimum and maximum coordinates
        double minX = Math.min(rect1.getBottomLeft().getX(), rect2.getBottomLeft().getX());
        double minY = Math.min(rect1.getBottomLeft().getY(), rect2.getBottomLeft().getY());
        double maxX = Math.max(rect1.getTopRight().getX(), rect2.getTopRight().getX());
        double maxY = Math.max(rect1.getTopRight().getY(), rect2.getTopRight().getY());
        
        // Create a new rectangle with these coordinates
        double width = maxX - minX;
        double height = maxY - minY;
        return new Rectangle(width, height, minX, minY);
    }
    
    /**
     * Scale a rectangle by a factor around its center.
     * @param rect The rectangle to scale.
     * @param factor The scaling factor.
     */
    public static void scaleRectangle(Rectangle rect, double factor) {
        Point center = rect.getCenter();
        double newWidth = rect.getWidth() * factor;
        double newHeight = rect.getHeight() * factor;
        double newX = center.getX() - newWidth / 2;
        double newY = center.getY() - newHeight / 2;
        
        // Create a new rectangle with the scaled dimensions
        Rectangle scaledRect = new Rectangle(newWidth, newHeight, newX, newY);
        
        // Update the original rectangle
        rect.setWidth(newWidth);
        rect.setHeight(newHeight);
        rect.setBottomLeft(new Point(newX, newY));
        rect.setBottomRight(new Point(newX + newWidth, newY));
        rect.setTopLeft(new Point(newX, newY + newHeight));
        rect.setTopRight(new Point(newX + newWidth, newY + newHeight));
    }
    
    /**
     * Check if a rectangle is a square.
     * @param rect The rectangle to check.
     * @return true if the rectangle is a square, false otherwise.
     */
    public static boolean isSquare(Rectangle rect) {
        return Math.abs(rect.getWidth() - rect.getHeight()) < 1e-10;
    }
    
    /**
     * Calculate the aspect ratio of a rectangle (width / height).
     * @param rect The rectangle.
     * @return The aspect ratio.
     */
    public static double aspectRatio(Rectangle rect) {
        return rect.getWidth() / rect.getHeight();
    }
    
    public static void main(String[] args) {
        Rectangle rect1 = new Rectangle(10, 5, 0, 0);
        Rectangle rect2 = new Rectangle(8, 4, 5, 2);
        
        // Calculate union area
        double unionArea = unionArea(rect1, rect2);
        System.out.println("Union area: " + unionArea);
        
        // Calculate bounding rectangle
        Rectangle boundingRect = boundingRectangle(rect1, rect2);
        System.out.println("Bounding rectangle: " + boundingRect);
        
        // Check if a rectangle is a square
        System.out.println("Is rectangle 1 a square? " + isSquare(rect1));
        
        // Calculate aspect ratio
        System.out.println("Aspect ratio of rectangle 1: " + aspectRatio(rect1));
        
        // Scale a rectangle
        scaleRectangle(rect1, 2.0);
        System.out.println("Rectangle 1 after scaling: " + rect1);
    }
}
``` 