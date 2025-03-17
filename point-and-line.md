# Point and Line

**Difficulty**: Easy  
**Tags**: Math, Geometry, Object-Oriented Design

## Question
Implement a geometric system that represents points and lines in a 2D plane. Your implementation should include:

1. A `Point` class that represents a point with x and y coordinates.
2. A `Line` class that represents a line defined by two points or by slope and y-intercept.
3. Methods to:
   - Calculate the distance between two points.
   - Determine if a point lies on a line.
   - Find the intersection point of two lines (if it exists).
   - Calculate the slope of a line.
   - Determine if two lines are parallel.
   - Determine if two lines are perpendicular.

## Example Input/Output
```
Point p1 = new Point(1, 2);
Point p2 = new Point(4, 6);

// Distance between points
double distance = p1.distanceTo(p2); // Returns 5.0

// Create a line from two points
Line line1 = new Line(p1, p2);
System.out.println(line1.getSlope()); // Returns 1.33...

// Check if a point is on the line
Point p3 = new Point(7, 10);
boolean isOnLine = line1.containsPoint(p3); // Returns true

// Create another line
Line line2 = new Line(new Point(2, 1), new Point(6, 3));

// Check if lines are parallel
boolean areParallel = line1.isParallelTo(line2); // Returns false

// Find intersection point
Point intersection = line1.intersectionWith(line2); // Returns Point(10, 14)
```

## Solution Algorithm
To implement this geometric system, we need to:

1. **Point Class**:
   - Store x and y coordinates.
   - Implement a method to calculate the distance between two points using the distance formula: d = √[(x₂ - x₁)² + (y₂ - y₁)²].

2. **Line Class**:
   - Represent a line in the form y = mx + b, where m is the slope and b is the y-intercept.
   - Alternatively, represent a line using two points.
   - Calculate the slope using the formula: m = (y₂ - y₁) / (x₂ - x₁).
   - Handle special cases like vertical lines (infinite slope).

3. **Line Methods**:
   - To check if a point lies on a line, substitute the point's coordinates into the line equation.
   - To find the intersection of two lines, solve the system of equations.
   - Two lines are parallel if they have the same slope.
   - Two lines are perpendicular if the product of their slopes is -1.

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
    
    /**
     * Calculate the distance to another point.
     * @param other The other point.
     * @return The distance between this point and the other point.
     */
    public double distanceTo(Point other) {
        double dx = other.x - this.x;
        double dy = other.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
    
    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Point point = (Point) obj;
        return Double.compare(point.x, x) == 0 && Double.compare(point.y, y) == 0;
    }
}

public class Line {
    private double slope;
    private double yIntercept;
    private boolean isVertical;
    private double xIntercept; // Only used for vertical lines
    
    /**
     * Create a line from two points.
     */
    public Line(Point p1, Point p2) {
        if (Math.abs(p2.getX() - p1.getX()) < 1e-10) {
            // Vertical line
            isVertical = true;
            xIntercept = p1.getX();
            slope = Double.POSITIVE_INFINITY;
        } else {
            isVertical = false;
            slope = (p2.getY() - p1.getY()) / (p2.getX() - p1.getX());
            yIntercept = p1.getY() - slope * p1.getX();
        }
    }
    
    /**
     * Create a line from slope and y-intercept.
     */
    public Line(double slope, double yIntercept) {
        this.slope = slope;
        this.yIntercept = yIntercept;
        this.isVertical = false;
    }
    
    /**
     * Create a vertical line with the given x-intercept.
     */
    public static Line createVerticalLine(double xIntercept) {
        Line line = new Line(0, 0);
        line.isVertical = true;
        line.xIntercept = xIntercept;
        line.slope = Double.POSITIVE_INFINITY;
        return line;
    }
    
    /**
     * Get the slope of the line.
     */
    public double getSlope() {
        return slope;
    }
    
    /**
     * Get the y-intercept of the line.
     */
    public double getYIntercept() {
        if (isVertical) {
            throw new UnsupportedOperationException("Vertical line has no y-intercept");
        }
        return yIntercept;
    }
    
    /**
     * Check if the line is vertical.
     */
    public boolean isVertical() {
        return isVertical;
    }
    
    /**
     * Get the x-intercept of the line.
     */
    public double getXIntercept() {
        if (isVertical) {
            return xIntercept;
        }
        return -yIntercept / slope;
    }
    
    /**
     * Check if a point lies on the line.
     */
    public boolean containsPoint(Point p) {
        if (isVertical) {
            return Math.abs(p.getX() - xIntercept) < 1e-10;
        }
        return Math.abs(p.getY() - (slope * p.getX() + yIntercept)) < 1e-10;
    }
    
    /**
     * Check if this line is parallel to another line.
     */
    public boolean isParallelTo(Line other) {
        if (isVertical && other.isVertical) {
            return true;
        }
        if (isVertical || other.isVertical) {
            return false;
        }
        return Math.abs(this.slope - other.slope) < 1e-10;
    }
    
    /**
     * Check if this line is perpendicular to another line.
     */
    public boolean isPerpendicularTo(Line other) {
        if (isVertical) {
            return Math.abs(other.slope) < 1e-10; // Other line is horizontal
        }
        if (other.isVertical) {
            return Math.abs(this.slope) < 1e-10; // This line is horizontal
        }
        return Math.abs(this.slope * other.slope + 1) < 1e-10;
    }
    
    /**
     * Find the intersection point of this line with another line.
     * @return The intersection point, or null if the lines are parallel.
     */
    public Point intersectionWith(Line other) {
        if (isParallelTo(other)) {
            return null; // Parallel lines don't intersect
        }
        
        if (isVertical) {
            double y = other.slope * xIntercept + other.yIntercept;
            return new Point(xIntercept, y);
        }
        
        if (other.isVertical) {
            double y = slope * other.xIntercept + yIntercept;
            return new Point(other.xIntercept, y);
        }
        
        // Solve the system of equations:
        // y = slope1 * x + yIntercept1
        // y = slope2 * x + yIntercept2
        double x = (other.yIntercept - yIntercept) / (slope - other.slope);
        double y = slope * x + yIntercept;
        
        return new Point(x, y);
    }
    
    @Override
    public String toString() {
        if (isVertical) {
            return "x = " + xIntercept;
        }
        return "y = " + slope + "x + " + yIntercept;
    }
    
    public static void main(String[] args) {
        Point p1 = new Point(1, 2);
        Point p2 = new Point(4, 6);
        
        // Distance between points
        double distance = p1.distanceTo(p2);
        System.out.println("Distance between " + p1 + " and " + p2 + ": " + distance);
        
        // Create a line from two points
        Line line1 = new Line(p1, p2);
        System.out.println("Line 1: " + line1);
        System.out.println("Slope of line 1: " + line1.getSlope());
        
        // Check if a point is on the line
        Point p3 = new Point(7, 10);
        boolean isOnLine = line1.containsPoint(p3);
        System.out.println("Is " + p3 + " on line 1? " + isOnLine);
        
        // Create another line
        Line line2 = new Line(new Point(2, 1), new Point(6, 3));
        System.out.println("Line 2: " + line2);
        
        // Check if lines are parallel
        boolean areParallel = line1.isParallelTo(line2);
        System.out.println("Are lines parallel? " + areParallel);
        
        // Check if lines are perpendicular
        boolean arePerpendicular = line1.isPerpendicularTo(line2);
        System.out.println("Are lines perpendicular? " + arePerpendicular);
        
        // Find intersection point
        Point intersection = line1.intersectionWith(line2);
        System.out.println("Intersection point: " + intersection);
        
        // Test vertical line
        Line verticalLine = Line.createVerticalLine(3);
        System.out.println("Vertical line: " + verticalLine);
        Point intersectionWithVertical = line1.intersectionWith(verticalLine);
        System.out.println("Intersection with vertical line: " + intersectionWithVertical);
    }
}
```

```java
// Alternative implementation with additional features
public class GeometryUtils {
    /**
     * Calculate the area of a triangle given three points.
     */
    public static double triangleArea(Point p1, Point p2, Point p3) {
        // Using the formula: Area = 0.5 * |x1(y2 - y3) + x2(y3 - y1) + x3(y1 - y2)|
        double area = 0.5 * Math.abs(
            p1.getX() * (p2.getY() - p3.getY()) +
            p2.getX() * (p3.getY() - p1.getY()) +
            p3.getX() * (p1.getY() - p2.getY())
        );
        return area;
    }
    
    /**
     * Check if three points are collinear (lie on the same line).
     */
    public static boolean areCollinear(Point p1, Point p2, Point p3) {
        // Three points are collinear if the area of the triangle formed by them is zero
        return triangleArea(p1, p2, p3) < 1e-10;
    }
    
    /**
     * Calculate the distance from a point to a line.
     */
    public static double distanceFromPointToLine(Point p, Line line) {
        if (line.isVertical()) {
            return Math.abs(p.getX() - line.getXIntercept());
        }
        
        // Distance = |ax + by + c| / sqrt(a² + b²)
        // For a line y = mx + b, we can rewrite as -mx + y - b = 0
        double a = -line.getSlope();
        double b = 1;
        double c = -line.getYIntercept();
        
        return Math.abs(a * p.getX() + b * p.getY() + c) / Math.sqrt(a * a + b * b);
    }
    
    /**
     * Find the midpoint between two points.
     */
    public static Point midpoint(Point p1, Point p2) {
        return new Point((p1.getX() + p2.getX()) / 2, (p1.getY() + p2.getY()) / 2);
    }
    
    /**
     * Calculate the angle between three points (angle at p2).
     * @return The angle in degrees.
     */
    public static double angleBetweenPoints(Point p1, Point p2, Point p3) {
        double angle1 = Math.atan2(p1.getY() - p2.getY(), p1.getX() - p2.getX());
        double angle2 = Math.atan2(p3.getY() - p2.getY(), p3.getX() - p2.getX());
        
        double angle = Math.abs(angle1 - angle2) * 180 / Math.PI;
        return angle > 180 ? 360 - angle : angle;
    }
    
    public static void main(String[] args) {
        Point p1 = new Point(0, 0);
        Point p2 = new Point(4, 0);
        Point p3 = new Point(0, 3);
        
        // Calculate triangle area
        double area = triangleArea(p1, p2, p3);
        System.out.println("Area of triangle: " + area);
        
        // Check if points are collinear
        Point p4 = new Point(2, 0);
        boolean collinear = areCollinear(p1, p2, p4);
        System.out.println("Are p1, p2, p4 collinear? " + collinear);
        
        // Calculate distance from point to line
        Line line = new Line(p1, p2); // Line along x-axis
        double distance = distanceFromPointToLine(p3, line);
        System.out.println("Distance from " + p3 + " to line: " + distance);
        
        // Find midpoint
        Point mid = midpoint(p1, p2);
        System.out.println("Midpoint of " + p1 + " and " + p2 + ": " + mid);
        
        // Calculate angle
        double angle = angleBetweenPoints(p2, p1, p3);
        System.out.println("Angle at " + p1 + ": " + angle + " degrees");
    }
}
``` 