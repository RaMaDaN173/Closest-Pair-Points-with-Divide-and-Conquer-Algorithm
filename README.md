# Closest-Pair-Points-with-Divide-and-Conquer-Algorithm

The Closest Pair of Points problem is a classic problem in computational geometry, and the Divide and Conquer algorithm is one of the most efficient ways to solve it. The problem is defined as follows: Given a set of points in a 2D plane, find the pair of points with the smallest Euclidean distance between them.

Here's an outline of the Divide and Conquer algorithm for solving the Closest Pair of Points problem:

1. Sort the points by their x-coordinates:
  * This step helps in reducing the search space efficiently.
2. Divide the set into two halves:
   * Divide the set of points into two equal halves based on the median x-coordinate. This step is crucial for the divide and conquer strategy.

3. Recursively find the closest pair in each half:
   * Use the algorithm recursively on the left and right halves to find the closest pair of points in each half.
     
4. Find the closest pair across the two halves:
   * Now, consider pairs where one point is in the left half and the other is in the right half. There can be points close to the dividing line that might form the closest pair.
  
5. Merge step:

   * Combine the results from the left and right halves and find the closest pair overall.
     
6. Update the minimum distance:

   * Finally, update the minimum distance by comparing it with the minimum distance obtained from the left and right halves and the distance across the dividing line

This algorithm ensures that we only consider points that are close to each other in the divided halves and efficiently narrows down the search space.

## Explanation of the code
---
This Java code implements the closest pair of points problem using the divide and conquer algorithm. Here's a breakdown of the code:

### Point Class:
   * Represents a 2D point with x and y coordinates.
   * Provides methods to get and set x and y values.

### PointPair Class:
   * Represents a pair of points (p, q).
   * Provides methods to get and set the points, and calculates the distance between them.

### Closest_Pair_Points Class:
   * Contains the main method for testing the closest pair algorithm.
   * Creates six points and adds them to a list ListOfPoint.
   * Calls the DivideAndConquer method to find the closest pair of points and prints the result.

`DivideAndConquer Method`:
   * Sorts the list of points based on their x-coordinates.
   * Calls the DivideAndConquerAUX method with the sorted list.

`DivideAndConquerAUX Method`:
   * Implements the divide and conquer algorithm.
   * If the number of points is small (<= 3), it uses the BruteForce method to find the closest pair.
   * Otherwise, it recursively divides the list into two halves and finds the closest pair in each half.
   * Finds the minimum distance among the pairs from the two halves.
   * Constructs a strip containing points within the minimum distance from the midpoint.
   * Calls the BestWithStrip method with the strip and the best pair so far.

`BruteForce Method`:
   * Computes the closest pair of points in the list using a brute-force approach.
   * Compares the distances between all pairs and returns the closest pair.

`BestWithStrip Method`:
   * Finds the closest pair of points within a given strip.
   * Sorts the strip based on y-coordinates.
   * Checks pairs within a certain y-coordinate range and updates the best pair if a closer pair is found.

**Note:**
   * The code prints the best pair of points and their distance in the BestWithStrip method.
   * The main method creates a set of points and demonstrates how to use the DivideAndConquer method to find the closest pair.

Make sure to test the code with different sets of points to observe how the algorithm performs.

# Code
## Point Class:
    public class Point {
    private int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    public int getY() {
        return y;
    }

    public void setY(int y) {
        this.y = y;
    }
    }

---
## PointPair Class:
    public class PointPair {
    private Point p, q;

    public PointPair(Point p, Point q) {
        this.p = p;
        this.q = q;
    }

    public Point getP() {
        return p;
    }

    public void setP(Point p) {
        this.p = p;
    }

    public Point getQ() {
        return q;
    }

    public void setQ(Point q) {
        this.q = q;
    }

    public double distance() {
        return Math.sqrt(Math.pow(p.getX() - q.getX(), 2) + Math.pow(p.getY() - q.getY(), 2));
    }
    }
    
---
## Closest_Pair_Points Class: & main Method

    import java.util.*;

    public class Closest_Pair_Points {
    public static void main(String[] args) {
        Point p1 = new Point(2,5);
        Point p2 = new Point(3,4);
        Point p3 = new Point(6,3);
        Point p4 = new Point(9,3);
        Point p5 = new Point(5,6);
        Point p6 = new Point(8,1);
        List<Point> ListOfPoint = new ArrayList<>();
        ListOfPoint.add(p1);
        ListOfPoint.add(p2);
        ListOfPoint.add(p3);
        ListOfPoint.add(p4);
        ListOfPoint.add(p5);
        ListOfPoint.add(p6);
        DivideAndConquer(ListOfPoint);
    }

    public static PointPair BruteForce(List<Point> ListOfPoints) {
        PointPair Best = new PointPair(ListOfPoints.get(0), ListOfPoints.get(1));
        for (int i = 2; i < ListOfPoints.size(); i++) {
            for (int j = i - 1; j >= 0; j--) {
                PointPair candidate = new PointPair(ListOfPoints.get(0), ListOfPoints.get(1));
                if (candidate.distance() < Best.distance())
                    Best = candidate;
            }
        }
        return Best;
    }

    public static PointPair DivideAndConquerAUX(List<Point> ListOfPoints) {
        List<Point> Strip;
        PointPair bestSoFar;
        if (ListOfPoints.size() <= 3) {
            return BruteForce(ListOfPoints);
        } else {
            int mid = ListOfPoints.size() / 2;
            Point MidPoint = ListOfPoints.get(mid);
            PointPair p1 = DivideAndConquerAUX(ListOfPoints.subList(0,mid));
            PointPair p2 = DivideAndConquerAUX(ListOfPoints.subList(mid,ListOfPoints.size()));
            bestSoFar = p1;
            if (p2.distance() < p1.distance()) {
                bestSoFar = p2;
            }
            Strip = new ArrayList<>();
            for (int i = 0; i < ListOfPoints.size(); i++) {
                if (Math.abs(ListOfPoints.get(i).getX() - MidPoint.getX()) < bestSoFar.distance()) {
                    Strip.add(ListOfPoints.get(i));
                }
            }
        }
        return BestWithStrip(Strip, bestSoFar);
    }

    public static PointPair DivideAndConquer(List<Point> points) {
        points.sort((o1, o2) -> Integer.signum(o1.getX() - o2.getX()));
        return DivideAndConquerAUX(points);
    }


    public static PointPair BestWithStrip(List<Point> Strip, PointPair Best) {
        PointPair best = Best;
        Strip.sort((o1, o2) -> Integer.signum(o1.getY() - o2.getY()));
        Point[] STRIP = Strip.toArray(new Point[0]);
        for (int i = 0; i < STRIP.length; i++) {
            for (int j = i + 1; j < STRIP.length && (STRIP[j].getY() - STRIP[i].getY()) <
                    best.distance(); j++) {
                PointPair candidate = new PointPair(STRIP[i], STRIP[j]);
                if (candidate.distance() < best.distance())
                    best = candidate;
            }
        }
        System.out.println("Best Pair is ( "+best.getP().getX()+" , "+best.getP().getY()+" ) & "+"( "+best.getQ().getX()+" , "+best.getQ().getY()+" )");
        System.out.println("With distance = "+best.distance());
        return best;
    }
    }
