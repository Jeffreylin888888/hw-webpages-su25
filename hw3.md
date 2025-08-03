### Part 1

- In my path tracer, rays are generated from the camera using its intrinsic properties (like the field of view) and resolution. For each pixel on the image plane, I compute a direction vector from the camera center through that pixel using the camera-to-world transformation. This gives me the primary ray for that pixel, and I cast that into the scene to look for intersections. Once a ray is fired, it traverses the bounding volume hierarchy (BVH), which accelerates the search for potential geometry intersections. If a BVH node’s bounding box is hit, I descend recursively; otherwise, that entire subtree is skipped. When I reach a leaf node, I test the ray against the actual geometry (triangles, spheres, etc.) inside. If I find the closest intersection, I store the hit point, surface normal, and material info for shading.
  
- To test if a ray intersects a triangle, I used the Möller-Trumbore algorithm. The idea is to treat the triangle using its vertices v0, v1, v2, and solve for barycentric coordinates (b1, b2) and a parameter t along the ray direction such that: P=O=tD= (1-b1-b2)*v0 + b1v1 + b2v2. We solve this using cross products and dot products to avoid explicitly computing the triangle plane equation. If all conditions hold (e.g. b1,b2 ≥ 0 b1+b2≤1, t>0), we return a hit with barycentric coordinates, interpolated normals, and the distance t. If not, the ray misses the triangle. This method is efficient and doesn’t require computing the triangle normal up front, which is perfect for tight loops in ray tracing.

- To demonstrate my normal shading implementation, I rendered three different .dae scenes using the -f flag to generate PNG files, as required. For CBspheres, I ran ./pathtracer -r 800 600 -f CBspheres.png ../dae/sky/CBspheres_lambertian.dae. This scene includes two spheres inside the Cornell Box, and the smooth color gradients across the curved surfaces confirm correct normal interpolation. For the banana scene, I used ./pathtracer -r 800 600 -f banana.png ../dae/keenan/banana.dae. The resulting image shows smoothly varying colors along the banana’s organic curves, validating the accuracy of my normal shading. Lastly, I rendered cow.dae using ./pathtracer -r 800 600 -f cow.png ../dae/sky/cow.dae. The cow’s rounded features and continuous shading transitions further showcase the successful interpolation of surface normals.


<table>
  <tr>
    <td align="center">
      <img src="assets/banana.png" width="200"/><br>banana.png
    </td>
    <td align="center">
      <img src="assets/CBspheres.png" width="200"/><br>CBspheres.png
    </td>
    <td align="center">
      <img src="assets/cow.png" width="200"/><br>cow.png
    </td>
  </tr>
</table>


### Part 2

### Part 2

<table>
  <tr>
    <td align="center">
      <img src="assets/cblucy.png" width="200"/><br>cblucy.png
    </td>
    <td align="center">
      <img src="assets/maxplanck_bvh.png" width="200"/><br>maxplanck_bvh.png
    </td>
    <td align="center">
      <img src="assets/cow_no_bvh.png" width="200"/><br>cow_no_bvh.png
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="assets/cbdragon.png" width="200"/><br>cbdragon.png
    </td>
    <td align="center">
      <img src="assets/CBbunny_screenshot_8-1_16-4-56.png.png" width="200"/><br>CBbunny_screenshot_8-1.png
    </td>
    <td align="center">
      <img src="assets/maxplanck_no_bvh.png" width="200"/><br>maxplanck_no_bvh.png
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="assets/maxplanck.png" width="200"/><br>maxplanck.png
    </td>
    <td></td>
    <td></td>
  </tr>
</table>

