---
layout: post
title: "Clustering Algorithms in Swift"
description: " "
date: 2023-09-25
tags: [Swift, ClusteringAlgorithms]
comments: true
share: true
---

Clustering is a fundamental task in machine learning and data analysis, where similar data points are grouped together based on their similarity. In this blog post, we will explore some popular clustering algorithms and how they can be implemented in Swift.

## 1. K-Means Clustering

K-means clustering is one of the most widely used clustering algorithms. It aims to partition a dataset into K clusters, where each data point belongs to the cluster with the nearest mean value. The algorithm works iteratively, optimizing the cluster centroids to minimize the sum of squared distances from each data point to its nearest centroid.

Here is an example implementation of K-means clustering in Swift:

```swift
import Foundation

struct Point {
    var x: Double
    var y: Double
}

func kmeans(data: [Point], k: Int) -> [[Point]] {
    var clusters = Array(repeating: [Point](), count: k)
    var centroids = Array(repeating: Point(x: 0, y: 0), count: k)

    // Initialize centroids randomly
    for i in 0..<k {
        centroids[i].x = Double.random(in: 0..<1)
        centroids[i].y = Double.random(in: 0..<1)
    }

    var isChanged = true
    while isChanged {
        // Assign data points to clusters
        for point in data {
            var minDistance = Double.infinity
            var clusterIdx = 0
            
            for (idx, centroid) in centroids.enumerated() {
                let distance = calculateDistance(point: point, centroid: centroid)
                if distance < minDistance {
                    minDistance = distance
                    clusterIdx = idx
                }
            }
            
            clusters[clusterIdx].append(point)
        }
        
        // Update centroids
        isChanged = false
        for (idx, cluster) in clusters.enumerated() {
            let newCentroid = calculateCentroid(cluster: cluster)
            if newCentroid != centroids[idx] {
                centroids[idx] = newCentroid
                isChanged = true
            }
        }
        
        // Clear clusters
        clusters = Array(repeating: [], count: k)
    }
    
    return clusters
}

func calculateDistance(point: Point, centroid: Point) -> Double {
    let deltaX = point.x - centroid.x
    let deltaY = point.y - centroid.y
    return sqrt(deltaX * deltaX + deltaY * deltaY)
}

func calculateCentroid(cluster: [Point]) -> Point {
    var sumX = 0.0
    var sumY = 0.0
    
    for point in cluster {
        sumX += point.x
        sumY += point.y
    }
    
    let centroidX = sumX / Double(cluster.count)
    let centroidY = sumY / Double(cluster.count)
    
    return Point(x: centroidX, y: centroidY)
}

// Test the K-means clustering algorithm
let data = [Point(x: 1, y: 1), Point(x: 2, y: 2), Point(x: 3, y: 3), Point(x: 10, y: 10), Point(x: 11, y: 11), Point(x: 12, y: 12)]
let k = 2
let clusters = kmeans(data: data, k: k)

for (idx, cluster) in clusters.enumerated() {
    print("Cluster \(idx + 1):")
    for point in cluster {
        print("(\(point.x), \(point.y))")
    }
    print("---")
}
```

## 2. Agglomerative Hierarchical Clustering

Agglomerative hierarchical clustering is another popular clustering algorithm that builds a hierarchy of clusters. It starts with each data point as an individual cluster and iteratively merges the closest clusters until a desired number of clusters is obtained. The algorithm uses a distance metric such as Euclidean distance to determine the similarity between clusters.

Here is an example implementation of Agglomerative Hierarchical Clustering in Swift:

```swift
import Foundation

struct Point {
    var x: Double
    var y: Double
}

struct Cluster {
    var points: [Point]
}

func hierarchicalClustering(data: [Point], k: Int) -> [Cluster] {
    var clusters = data.map { Cluster(points: [$0]) }
    
    while clusters.count > k {
        // Find the two closest clusters
        var minDistance = Double.infinity
        var closestClusters = (0, 0)
        
        for i in 0..<clusters.count {
            for j in i+1..<clusters.count {
                let distance = calculateDistance(cluster1: clusters[i], cluster2: clusters[j])
                if distance < minDistance {
                    minDistance = distance
                    closestClusters = (i, j)
                }
            }
        }
        
        // Merge the closest clusters
        let mergedClusterPoints = clusters[closestClusters.0].points + clusters[closestClusters.1].points
        let mergedCluster = Cluster(points: mergedClusterPoints)
        clusters.remove(at: closestClusters.1)
        clusters[closestClusters.0] = mergedCluster
    }
    
    return clusters
}

func calculateDistance(cluster1: Cluster, cluster2: Cluster) -> Double {
    var minDistance = Double.infinity
    
    for point1 in cluster1.points {
        for point2 in cluster2.points {
            let distance = calculateDistance(point: point1, centroid: point2)
            if distance < minDistance {
                minDistance = distance
            }
        }
    }
    
    return minDistance
}

func calculateDistance(point: Point, centroid: Point) -> Double {
    let deltaX = point.x - centroid.x
    let deltaY = point.y - centroid.y
    return sqrt(deltaX * deltaX + deltaY * deltaY)
}

// Test the Agglomerative Hierarchical Clustering algorithm
let data = [Point(x: 1, y: 1), Point(x: 2, y: 2), Point(x: 3, y: 3), Point(x: 10, y: 10), Point(x: 11, y: 11), Point(x: 12, y: 12)]
let k = 2
let clusters = hierarchicalClustering(data: data, k: k)

for (idx, cluster) in clusters.enumerated() {
    print("Cluster \(idx + 1):")
    for point in cluster.points {
        print("(\(point.x), \(point.y))")
    }
    print("---")
}
```

## Conclusion

In this blog post, we explored two popular clustering algorithms - K-means clustering and Agglomerative Hierarchical Clustering - and implemented them in Swift. Clustering algorithms are essential in various applications, such as data mining, image segmentation, and recommendation systems. By understanding these algorithms and their implementations, you can effectively group similar data points and discover meaningful patterns in your datasets. #Swift #ClusteringAlgorithms