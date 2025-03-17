# Triangle

**Difficulty**: Medium  
**Tags**: Math, Geometry, Object-Oriented Design

## Question
Implement a `Triangle` class that represents a triangle in a 2D plane. Your implementation should include:

1. A constructor that creates a triangle with three points (vertices).
2. Methods to:
   - Calculate the area of the triangle.
   - Calculate the perimeter of the triangle.
   - Determine if a point is inside the triangle.
   - Calculate the centroid of the triangle.
   - Calculate the circumcenter of the triangle.
   - Calculate the incenter of the triangle.
   - Calculate the orthocenter of the triangle.
   - Determine if the triangle is equilateral, isosceles, or scalene.
   - Determine if the triangle is acute, right, or obtuse.
   - Check if a given triangle intersects with this triangle.

## Example Input/Output
```
Point p1 = new Point(0, 0);
Point p2 = new Point(4, 0);
Point p3 = new Point(2, 3);
Triangle t = new Triangle(p1, p2, p3);

System.out.println(t.getArea()); // Returns 6.0
System.out.println(t.getPerimeter()); // Returns 12.0

Point p = new Point(2, 1);
System.out.println(t.containsPoint(p)); // Returns true

System.out.println(t.getCentroid()); // Returns (2.0, 1.0)
System.out.println(t.getCircumcenter()); // Returns (2.0, 1.5)
System.out.println(t.getIncenter()); // Returns (2.0, 1.0)
System.out.println(t.getOrthocenter()); // Returns (2.0, 0.0)

System.out.println(t.isEquilateral()); // Returns false
System.out.println(t.isIsosceles()); // Returns true
System.out.println(t.isScalene()); // Returns false

System.out.println(t.isAcute()); // Returns true
System.out.println(t.isRight()); // Returns false
System.out.println(t.isObtuse()); // Returns false

Triangle t2 = new Triangle(new Point(1, 1), new Point(5, 1), new Point(3, 4));
System.out.println(t.intersects(t2)); // Returns true
```

## Solution Algorithm
To implement the `Triangle` class, we need to:

1. Store the three vertices of the triangle.
2. Calculate area using the formula: A = (1/2) * |x1(y2 - y3) + x2(y3 - y1) + x3(y1 - y2)|.
3. Calculate perimeter as the sum of the lengths of the three sides.
4. To check if a point is inside the triangle, use the barycentric coordinate method.
5. Calculate the centroid as the average of the three vertices.
6. Calculate the circumcenter using the perpendicular bisectors of the sides.
7. Calculate the incenter using the angle bisectors of the vertices.
8. Calculate the orthocenter using the altitudes of the triangle.
9. Determine the triangle type (equilateral, isosceles, scalene) by comparing the lengths of the sides.
10. Determine the triangle angle type (acute, right, obtuse) using the Pythagorean theorem.
11. To check if two triangles intersect, check if any edge of one triangle intersects with any edge of the other triangle, or if one triangle contains a vertex of the other.

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
    
    public double distanceTo(Point other) {
        double dx = this.x - other.x;
        double dy = this.y - other.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
    
    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}

public class Triangle {
    private Point p1;
    private Point p2;
    private Point p3;
    
    /**
     * Create a triangle with the given vertices.
     * @param p1 The first vertex.
     * @param p2 The second vertex.
     * @param p3 The third vertex.
     */
    public Triangle(Point p1, Point p2, Point p3) {
        // Check if the points are collinear
        double area = 0.5 * Math.abs(
            p1.getX() * (p2.getY() - p3.getY()) +
            p2.getX() * (p3.getY() - p1.getY()) +
            p3.getX() * (p1.getY() - p2.getY())
        );
        
        if (area < 1e-10) {
            throw new IllegalArgumentException("The points are collinear and do not form a triangle");
        }
        
        this.p1 = p1;
        this.p2 = p2;
        this.p3 = p3;
    }
    
    /**
     * Get the first vertex of the triangle.
     * @return The first vertex.
     */
    public Point getP1() {
        return p1;
    }
    
    /**
     * Get the second vertex of the triangle.
     * @return The second vertex.
     */
    public Point getP2() {
        return p2;
    }
    
    /**
     * Get the third vertex of the triangle.
     * @return The third vertex.
     */
    public Point getP3() {
        return p3;
    }
    
    /**
     * Get the length of the first side (p1 to p2).
     * @return The length of the first side.
     */
    public double getSide1() {
        return p1.distanceTo(p2);
    }
    
    /**
     * Get the length of the second side (p2 to p3).
     * @return The length of the second side.
     */
    public double getSide2() {
        return p2.distanceTo(p3);
    }
    
    /**
     * Get the length of the third side (p3 to p1).
     * @return The length of the third side.
     */
    public double getSide3() {
        return p3.distanceTo(p1);
    }
    
    /**
     * Get the area of the triangle.
     * @return The area.
     */
    public double getArea() {
        return 0.5 * Math.abs(
            p1.getX() * (p2.getY() - p3.getY()) +
            p2.getX() * (p3.getY() - p1.getY()) +
            p3.getX() * (p1.getY() - p2.getY())
        );
    }
    
    /**
     * Get the perimeter of the triangle.
     * @return The perimeter.
     */
    public double getPerimeter() {
        return getSide1() + getSide2() + getSide3();
    }
    
    /**
     * Check if a point is inside the triangle.
     * @param p The point to check.
     * @return true if the point is inside the triangle, false otherwise.
     */
    public boolean containsPoint(Point p) {
        // Barycentric coordinate method
        double area = getArea();
        double area1 = new Triangle(p, p2, p3).getArea();
        double area2 = new Triangle(p1, p, p3).getArea();
        double area3 = new Triangle(p1, p2, p).getArea();
        
        // Check if the sum of the sub-triangle areas equals the original triangle area
        return Math.abs(area - (area1 + area2 + area3)) < 1e-10;
    }
    
    /**
     * Get the centroid of the triangle.
     * @return The centroid point.
     */
    public Point getCentroid() {
        double x = (p1.getX() + p2.getX() + p3.getX()) / 3;
        double y = (p1.getY() + p2.getY() + p3.getY()) / 3;
        return new Point(x, y);
    }
    
    /**
     * Get the circumcenter of the triangle.
     * @return The circumcenter point.
     */
    public Point getCircumcenter() {
        double d = 2 * (p1.getX() * (p2.getY() - p3.getY()) +
                        p2.getX() * (p3.getY() - p1.getY()) +
                        p3.getX() * (p1.getY() - p2.getY()));
        
        double x = ((p1.getX() * p1.getX() + p1.getY() * p1.getY()) * (p2.getY() - p3.getY()) +
                    (p2.getX() * p2.getX() + p2.getY() * p2.getY()) * (p3.getY() - p1.getY()) +
                    (p3.getX() * p3.getX() + p3.getY() * p3.getY()) * (p1.getY() - p2.getY())) / d;
        
        double y = ((p1.getX() * p1.getX() + p1.getY() * p1.getY()) * (p3.getX() - p2.getX()) +
                    (p2.getX() * p2.getX() + p2.getY() * p2.getY()) * (p1.getX() - p3.getX()) +
                    (p3.getX() * p3.getX() + p3.getY() * p3.getY()) * (p2.getX() - p1.getX())) / d;
        
        return new Point(x, y);
    }
    
    /**
     * Get the incenter of the triangle.
     * @return The incenter point.
     */
    public Point getIncenter() {
        double a = getSide1();
        double b = getSide2();
        double c = getSide3();
        
        double x = (a * p3.getX() + b * p1.getX() + c * p2.getX()) / (a + b + c);
        double y = (a * p3.getY() + b * p1.getY() + c * p2.getY()) / (a + b + c);
        
        return new Point(x, y);
    }
    
    /**
     * Get the orthocenter of the triangle.
     * @return The orthocenter point.
     */
    public Point getOrthocenter() {
        // Calculate the slopes of the sides
        double m1 = (p2.getY() - p1.getY()) / (p2.getX() - p1.getX() + 1e-10);
        double m2 = (p3.getY() - p2.getY()) / (p3.getX() - p2.getX() + 1e-10);
        double m3 = (p1.getY() - p3.getY()) / (p1.getX() - p3.getX() + 1e-10);
        
        // Calculate the slopes of the altitudes (perpendicular to the sides)
        double a1 = -1 / m1;
        double a2 = -1 / m2;
        double a3 = -1 / m3;
        
        // Calculate the y-intercepts of the altitudes
        double b1 = p3.getY() - a1 * p3.getX();
        double b2 = p1.getY() - a2 * p1.getX();
        
        // Calculate the intersection of two altitudes
        double x = (b2 - b1) / (a1 - a2);
        double y = a1 * x + b1;
        
        return new Point(x, y);
    }
    
    /**
     * Check if the triangle is equilateral.
     * @return true if the triangle is equilateral, false otherwise.
     */
    public boolean isEquilateral() {
        double side1 = getSide1();
        double side2 = getSide2();
        double side3 = getSide3();
        
        return Math.abs(side1 - side2) < 1e-10 && Math.abs(side2 - side3) < 1e-10;
    }
    
    /**
     * Check if the triangle is isosceles.
     * @return true if the triangle is isosceles, false otherwise.
     */
    public boolean isIsosceles() {
        double side1 = getSide1();
        double side2 = getSide2();
        double side3 = getSide3();
        
        return Math.abs(side1 - side2) < 1e-10 || Math.abs(side2 - side3) < 1e-10 || Math.abs(side3 - side1) < 1e-10;
    }
    
    /**
     * Check if the triangle is scalene.
     * @return true if the triangle is scalene, false otherwise.
     */
    public boolean isScalene() {
        return !isIsosceles();
    }
    
    /**
     * Check if the triangle is acute (all angles are less than 90 degrees).
     * @return true if the triangle is acute, false otherwise.
     */
    public boolean isAcute() {
        double side1 = getSide1();
        double side2 = getSide2();
        double side3 = getSide3();
        
        double side1Squared = side1 * side1;
        double side2Squared = side2 * side2;
        double side3Squared = side3 * side3;
        
        return side1Squared + side2Squared > side3Squared &&
               side2Squared + side3Squared > side1Squared &&
               side3Squared + side1Squared > side2Squared;
    }
    
    /**
     * Check if the triangle is right (one angle is exactly 90 degrees).
     * @return true if the triangle is right, false otherwise.
     */
    public boolean isRight() {
        double side1 = getSide1();
        double side2 = getSide2();
        double side3 = getSide3();
        
        double side1Squared = side1 * side1;
        double side2Squared = side2 * side2;
        double side3Squared = side3 * side3;
        
        return Math.abs(side1Squared + side2Squared - side3Squared) < 1e-10 ||
               Math.abs(side2Squared + side3Squared - side1Squared) < 1e-10 ||
               Math.abs(side3Squared + side1Squared - side2Squared) < 1e-10;
    }
    
    /**
     * Check if the triangle is obtuse (one angle is greater than 90 degrees).
     * @return true if the triangle is obtuse, false otherwise.
     */
    public boolean isObtuse() {
        return !isAcute() && !isRight();
    }
    
    /**
     * Check if this triangle intersects with another triangle.
     * @param other The other triangle.
     * @return true if the triangles intersect, false otherwise.
     */
    public boolean intersects(Triangle other) {
        // Check if any edge of this triangle intersects with any edge of the other triangle
        if (edgesIntersect(this, other)) {
            return true;
        }
        
        // Check if any vertex of one triangle is inside the other triangle
        return containsPoint(other.p1) || containsPoint(other.p2) || containsPoint(other.p3) ||
               other.containsPoint(p1) || other.containsPoint(p2) || other.containsPoint(p3);
    }
    
    /**
     * Check if any edge of one triangle intersects with any edge of another triangle.
     * @param t1 The first triangle.
     * @param t2 The second triangle.
     * @return true if any edges intersect, false otherwise.
     */
    private boolean edgesIntersect(Triangle t1, Triangle t2) {
        // Check all possible edge pairs
        return lineSegmentsIntersect(t1.p1, t1.p2, t2.p1, t2.p2) ||
               lineSegmentsIntersect(t1.p1, t1.p2, t2.p2, t2.p3) ||
               lineSegmentsIntersect(t1.p1, t1.p2, t2.p3, t2.p1) ||
               lineSegmentsIntersect(t1.p2, t1.p3, t2.p1, t2.p2) ||
               lineSegmentsIntersect(t1.p2, t1.p3, t2.p2, t2.p3) ||
               lineSegmentsIntersect(t1.p2, t1.p3, t2.p3, t2.p1) ||
               lineSegmentsIntersect(t1.p3, t1.p1, t2.p1, t2.p2) ||
               lineSegmentsIntersect(t1.p3, t1.p1, t2.p2, t2.p3) ||
               lineSegmentsIntersect(t1.p3, t1.p1, t2.p3, t2.p1);
    }
    
    /**
     * Check if two line segments intersect.
     * @param p1 The start point of the first line segment.
     * @param p2 The end point of the first line segment.
     * @param p3 The start point of the second line segment.
     * @param p4 The end point of the second line segment.
     * @return true if the line segments intersect, false otherwise.
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
     * Calculate the cross product (p2 - p1) × (p3 - p1).
     */
    private double crossProduct(Point p1, Point p2, Point p3) {
        return (p2.getX() - p1.getX()) * (p3.getY() - p1.getY()) - 
               (p2.getY() - p1.getY()) * (p3.getX() - p1.getX());
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
    
    @Override
    public String toString() {
        return "Triangle[" + p1 + ", " + p2 + ", " + p3 + "]";
    }
    
    public static void main(String[] args) {
        // Create a triangle
        Point p1 = new Point(0, 0);
        Point p2 = new Point(4, 0);
        Point p3 = new Point(2, 3);
        Triangle t = new Triangle(p1, p2, p3);
        
        System.out.println("Triangle: " + t);
        System.out.println("Area: " + t.getArea());
        System.out.println("Perimeter: " + t.getPerimeter());
        
        // Check if a point is inside the triangle
        Point p = new Point(2, 1);
        System.out.println("Is point " + p + " inside the triangle? " + t.containsPoint(p));
        
        // Calculate the centers
        System.out.println("Centroid: " + t.getCentroid());
        System.out.println("Circumcenter: " + t.getCircumcenter());
        System.out.println("Incenter: " + t.getIncenter());
        System.out.println("Orthocenter: " + t.getOrthocenter());
        
        // Check the triangle type
        System.out.println("Is equilateral? " + t.isEquilateral());
        System.out.println("Is isosceles? " + t.isIsosceles());
        System.out.println("Is scalene? " + t.isScalene());
        
        // Check the angle type
        System.out.println("Is acute? " + t.isAcute());
        System.out.println("Is right? " + t.isRight());
        System.out.println("Is obtuse? " + t.isObtuse());
        
        // Create another triangle
        Triangle t2 = new Triangle(new Point(1, 1), new Point(5, 1), new Point(3, 4));
        System.out.println("Do the triangles intersect? " + t.intersects(t2));
    }
}
```

```java
// Alternative implementation with additional features
public class TriangleUtils {
    /**
     * Calculate the altitude from a vertex to the opposite side.
     * @param t The triangle.
     * @param vertex The vertex (1, 2, or 3).
     * @return The altitude length.
     */
    public static double altitude(Triangle t, int vertex) {
        double area = t.getArea();
        double base;
        
        switch (vertex) {
            case 1:
                base = t.getP2().distanceTo(t.getP3());
                break;
            case 2:
                base = t.getP3().distanceTo(t.getP1());
                break;
            case 3:
                base = t.getP1().distanceTo(t.getP2());
                break;
            default:
                throw new IllegalArgumentException("Vertex must be 1, 2, or 3");
        }
        
        return 2 * area / base;
    }
    
    /**
     * Calculate the angle at a vertex.
     * @param t The triangle.
     * @param vertex The vertex (1, 2, or 3).
     * @return The angle in radians.
     */
    public static double angle(Triangle t, int vertex) {
        Point p1, p2, p3;
        
        switch (vertex) {
            case 1:
                p1 = t.getP1();
                p2 = t.getP2();
                p3 = t.getP3();
                break;
            case 2:
                p1 = t.getP2();
                p2 = t.getP3();
                p3 = t.getP1();
                break;
            case 3:
                p1 = t.getP3();
                p2 = t.getP1();
                p3 = t.getP2();
                break;
            default:
                throw new IllegalArgumentException("Vertex must be 1, 2, or 3");
        }
        
        double a = p1.distanceTo(p2);
        double b = p1.distanceTo(p3);
        double c = p2.distanceTo(p3);
        
        return Math.acos((a * a + b * b - c * c) / (2 * a * b));
    }
    
    /**
     * Calculate the median from a vertex to the midpoint of the opposite side.
     * @param t The triangle.
     * @param vertex The vertex (1, 2, or 3).
     * @return The median length.
     */
    public static double median(Triangle t, int vertex) {
        Point p1, p2, p3;
        
        switch (vertex) {
            case 1:
                p1 = t.getP1();
                p2 = t.getP2();
                p3 = t.getP3();
                break;
            case 2:
                p1 = t.getP2();
                p2 = t.getP3();
                p3 = t.getP1();
                break;
            case 3:
                p1 = t.getP3();
                p2 = t.getP1();
                p3 = t.getP2();
                break;
            default:
                throw new IllegalArgumentException("Vertex must be 1, 2, or 3");
        }
        
        // Calculate the midpoint of the opposite side
        double midX = (p2.getX() + p3.getX()) / 2;
        double midY = (p2.getY() + p3.getY()) / 2;
        Point midpoint = new Point(midX, midY);
        
        return p1.distanceTo(midpoint);
    }
    
    /**
     * Calculate the circumradius of the triangle.
     * @param t The triangle.
     * @return The circumradius.
     */
    public static double circumradius(Triangle t) {
        double a = t.getSide1();
        double b = t.getSide2();
        double c = t.getSide3();
        
        return (a * b * c) / (4 * t.getArea());
    }
    
    /**
     * Calculate the inradius of the triangle.
     * @param t The triangle.
     * @return The inradius.
     */
    public static double inradius(Triangle t) {
        return 2 * t.getArea() / t.getPerimeter();
    }
    
    public static void main(String[] args) {
        Point p1 = new Point(0, 0);
        Point p2 = new Point(4, 0);
        Point p3 = new Point(2, 3);
        Triangle t = new Triangle(p1, p2, p3);
        
        // Calculate the altitude from each vertex
        System.out.println("Altitude from vertex 1: " + altitude(t, 1));
        System.out.println("Altitude from vertex 2: " + altitude(t, 2));
        System.out.println("Altitude from vertex 3: " + altitude(t, 3));
        
        // Calculate the angle at each vertex
        System.out.println("Angle at vertex 1: " + Math.toDegrees(angle(t, 1)) + " degrees");
        System.out.println("Angle at vertex 2: " + Math.toDegrees(angle(t, 2)) + " degrees");
        System.out.println("Angle at vertex 3: " + Math.toDegrees(angle(t, 3)) + " degrees");
        
        // Calculate the median from each vertex
        System.out.println("Median from vertex 1: " + median(t, 1));
        System.out.println("Median from vertex 2: " + median(t, 2));
        System.out.println("Median from vertex 3: " + median(t, 3));
        
        // Calculate the circumradius and inradius
        System.out.println("Circumradius: " + circumradius(t));
        System.out.println("Inradius: " + inradius(t));
    }
}
``` 