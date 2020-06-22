<!--

@created at 2020-06-22

#
#
#

-->

# Contents

- [Contents](#contents)
- [Algorithms](#algorithms)
  - [bilateral filter](#bilateral-filter)
    - [bilateral filter implementation](#bilateral-filter-implementation)

<!--

- <div align="left"><img src="xxx" height="" width="640" /></div>

#
#
#

<details>
<summary>
</summary>
<br/>
</details>

-->

# Algorithms

## bilateral filter

- [C. Tomasi and R. Manduchi. Bilateral Filtering for Gray and Color Images. In Proceedings of the IEEE International Conference on Computer Vision, 1998.](http://www.super.tka4.org/materials/lib/Articles-Books/Filters/Bilateral/tomasi98bilateral.pdf)

- [Python | Bilateral Filtering](https://www.geeksforgeeks.org/python-bilateral-filtering/)

A bilteral filter is used for smoothing images and reducing noise, while preserving edges.

- `Gaussian Blur`

  $$\Huge{
    GB[I]_p = \sum\limits_{q \in S} G_{\sigma} (\Bigg \Vert p - q \Bigg \Vert) I_q
  }$$

  - Normalized Gaussion Function: $\Huge{G_{\sigma} (\Bigg \Vert p - q \Bigg \Vert)}$
  - Here, $\Huge{GB[I]_p}$ is the result at pixel $\Huge{p}$, and the $\Huge{RMS}$ is
    essentially a sum over all pixels $\Huge{q}$ weighted by the Gaussion function.
    $\Huge{I_q}$ is the intensity at pixel $\Huge{q}$.

- `Bilateral Filter: an Additional Edge Term`

  $$\Huge{
    BF[I]_p = \frac{1}{W_p} \sum\limits_{q \in S} G_{\sigma_s} (\Bigg \Vert p - q \Bigg \Vert) G_{\sigma_r} (\Bigg \vert I_p - I_q \Bigg \vert) I_q
  }$$

  - Normalization Factor: $\Huge{\frac{1}{W_q}}$
  - Space Weight: $\Huge{G_{\sigma_s} (\Bigg \Vert p - q \Bigg \Vert)}$
  - Range Weight: $\Huge{G_{\sigma_r} (\Bigg \vert I_p - I_q \Bigg \vert)}$
  - $\Huge{\sigma_s}$ denotes the spatial extent of the kernel, i.e. the size of the neighbourhood.
  - $\Huge{\sigma_r}$ denotes the minimum amplitude (振幅) of an edge.
    - The smaller value of $\Huge{\sigma_r}$, the sharper the edge.
    - As $\Huge{\sigma_r}$ tends to infinity, the equation tends to a Gaussian blur.

<!--

#
#
#

-->

### bilateral filter implementation

```bash
tree -f -a | grep -i bilateral
│   │           ├── ./filters/include/pcl/filters/bilateral.h
│   │           ├── ./filters/include/pcl/filters/fast_bilateral.h
│   │           ├── ./filters/include/pcl/filters/fast_bilateral_omp.h
│   │           │   ├── ./filters/include/pcl/filters/impl/bilateral.hpp
│   │           │   ├── ./filters/include/pcl/filters/impl/fast_bilateral.hpp
│   │           │   ├── ./filters/include/pcl/filters/impl/fast_bilateral_omp.hpp
│       ├── ./filters/src/bilateral.cpp
│       ├── ./filters/src/fast_bilateral.cpp
│       ├── ./filters/src/fast_bilateral_omp.cpp
│   │   │   │   ├── ./gpu/kinfu/src/cuda/bilateral_pyrdown.cu
│   │   │   │   ├── ./gpu/kinfu_large_scale/src/cuda/bilateral_pyrdown.cu
│   │           ├── ./surface/include/pcl/surface/bilateral_upsampling.h
│   │           │   ├── ./surface/include/pcl/surface/impl/bilateral_upsampling.hpp
│   │   ├── ./surface/src/bilateral_upsampling.cpp
│   │   ├── ./test/filters/test_bilateral.cpp
│   ├── ./tools/bilateral_upsampling.cpp
│   ├── ./tools/fast_bilateral_filter.cpp
```

- [bilateral.h](./filters/include/pcl/filters/bilateral.h)

  ```C++
  template<typename PointT>
  class BilateralFilter : public Filter<PointT>
  ```

- [bilateral.hpp](./filters/include/pcl/filters/impl/bilateral.hpp)

  ```C++
  template <typename PointT> double
  pcl::BilateralFilter<PointT>::computePointWeight(
      const int pid,
      const std::vector<int> &indices,
      const std::vector<float> &distances
  )
  ```

- [fast_bilateral.h](./filters/include/pcl/filters/fast_bilateral.h)

  ```C++
  template<typename PointT>
  class FastBilateralFilter : public Filter<PointT> {
      FastBilateralFilter()
         : sigma_s_ (15.0f)
         , sigma_r_ (0.05f)
         , early_division_ (false) {}
  };
  ```

- [fast_bilateral.hpp](./filters/include/pcl/filters/impl/fast_bilateral.hpp)
- [fast_bilateral_omp.h](./filters/include/pcl/filters/fast_bilateral_omp.h)
- [fast_bilateral_omp.hpp](./filters/include/pcl/filters/impl/fast_bilateral_omp.hpp)

- [test_bilateral.cpp](./test/filters/test_bilateral.cpp)

  ```C++
  FastBilateralFilter<PointXYZ> fbf;
  fbf.setSigmaS( 5 );
  fbs.setSigmaR( 0.03f );
  ```

- [fast_bilateral_filter.cpp](./tools/fast_bilateral_filter.cpp)

  ```C++
  float default_sigma_s = 5.0f;
  float default_sigma_r = 0.03f;
  ```

<!--

- <div align="left"><img src="xxx" height="" width="640" /></div>

#
#
#

<details>
<summary>
</summary>
<br/>
</details>

-->
