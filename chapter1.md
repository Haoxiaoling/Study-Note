# PCL 3D feature

![](/assets/import.png)

* **setIndices\(\) = false, setSearchSurface\(\) = false**- this is without a doubt the most used case in PCL, where the user is just feeding in a single PointCloud dataset and expects a certain feature estimated at_all the points in the cloud_.

  Since we do not expect to maintain different implementation copies based on whether a set of indices and/or the search surface is given, whenever indices = false , PCL creates a set of internal indices \(as astd::vector&lt;int&gt;\) that basically point to the entire dataset \(indices=1..N, where N is the number of points in the cloud\).

  In the figure above, this corresponds to the leftmost case. First, we estimate the nearest neighbors of p\_1, then the nearest neighbors of p\_2, and so on, until we exhaust all the points in P.

* **setIndices\(\) = true, setSearchSurface\(\) = false**- as previously mentioned, the feature estimation method will only compute features for those points which have an index in the given indices vector;

  In the figure above, this corresponds to the second case. Here, we assume that p\_2’s index is not part of the indices vector given, so no neighbors or features will be estimated at p2.

* **setIndices\(\) = false, setSearchSurface\(\) = true**- as in the first case, features will be estimated for all points given as input, but, the underlying neighboring surface given in**setSearchSurface\(\)**will be used to obtain nearest neighbors for the input points, rather than the input cloud itself;

  In the figure above, this corresponds to the third case. If Q={q\_1, q\_2} is another cloud given as input, different than P, and P is the search surface for Q, then the neighbors of q\_1 and q\_2 will be computed from P.

* **setIndices\(\) = true, setSearchSurface\(\) = true**- this is probably the rarest case, where both indices and a search surface is given. In this case, features will be estimated for only a subset from the &lt;input, indices&gt; pair, using the search surface information given in**setSearchSurface\(\)**.

  Finally, un the figure above, this corresponds to the last \(rightmost\) case. Here, we assume that q\_2’s index is not part of the indices vector given for Q, so no neighbors or features will be estimated at q2.

  The most useful example when**setSearchSurface\(\)**should be used, is
  when we have a very dense input dataset, but we do not want to estimate features at all the points in it, but rather at some keypoints discovered using the methods inpcl\_keypoints, or at a downsampled version of the cloud \(e.g., obtained using apcl::VoxelGrid&lt;T&gt;filter\). In this case, we pass the downsampled/keypoints input via**setInputCloud\(\)**, and the original data as**setSearchSurface\(\)**.
