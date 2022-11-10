# oppcatchera.github.io
<!DOCTYPE html>
<html lang="en-US">

  <head>
    <meta charset='utf-8'>
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,maximum-scale=2">
    <link rel="stylesheet" type="text/css" media="screen" href="/DelaunayTriangulation/assets/css/style.css?v=d4100040f2a64f28a6ba478a53ed13cd34b54216">

<!-- Begin Jekyll SEO tag v2.5.0 -->
<title>Delaunay Triangulation | DelaunayTriangulation</title>
<meta name="generator" content="Jekyll v3.7.4" />
<meta property="og:title" content="Delaunay Triangulation" />
<meta property="og:locale" content="en_US" />
<link rel="canonical" href="https://conniefan.github.io/DelaunayTriangulation/" />
<meta property="og:url" content="https://conniefan.github.io/DelaunayTriangulation/" />
<meta property="og:site_name" content="DelaunayTriangulation" />
<script type="application/ld+json">
{"@type":"WebSite","headline":"Delaunay Triangulation","url":"https://conniefan.github.io/DelaunayTriangulation/","name":"DelaunayTriangulation","@context":"http://schema.org"}</script>
<!-- End Jekyll SEO tag -->

  </head>

  <body>

    <!-- HEADER -->
    <div id="header_wrap" class="outer">
        <header class="inner">
          <a id="forkme_banner" href="https://github.com/conniefan/DelaunayTriangulation">View on GitHub</a>

          <h1 id="project_title">DelaunayTriangulation</h1>
          <h2 id="project_tagline"></h2>

          
        </header>
    </div>

    <!-- MAIN CONTENT -->
    <div id="main_content_wrap" class="outer">
      <section id="main_content" class="inner">
        <h1 id="delaunay-triangulation">Delaunay Triangulation</h1>

<p>Harvey Zhang and Connie Fan</p>

<p>In this project, we explored the parallelization of Delaunay Triangulation algorithms using CUDA on the GPU. Specifically, we analyzed the performance of four GPU implementations based on Voronoi diagrams. For sufficiently large input sizes, our Jump Flood GPU implementation achieves a significantly higher speedup over a single-threaded CPU Randomized Incremental implementation[1]. For an input set of 10 million points, our best implementation has a 70x speedup.</p>

<p><a href="/DelaunayTriangulation/Parallel_Delaunay_Triangulation_on_CUDA.pdf">Final paper</a></p>

<p><a href="/DelaunayTriangulation/checkpoint.html">Checkpoint</a></p>

<p><a href="/DelaunayTriangulation/proposal.html">Proposal</a></p>

<h2 id="background">Background</h2>
<p>Delaunay Triangulation (DT) is an algorithm that computes a triangulation of a set of points or
vertices such that no vertex is within the circumcircle of any triangle in the resulting triangulation.
Incremental DT attempts to insert a new vertex into an existing set of triangulation by determining the
triangles that violate the Delaunay condition. The DT algorithm is generally computation-heavy and
several components of the algorithm may see significant speedups from parallelization. For example,
the incremental algorithm can be parallelized by allowing for parallel/concurrent insertions into the
existing set of triangles. However, implementing such parallelization schemes may not be trivial [2].</p>

<p>Delaunay Triangulation, in general, can be applied to areas such as 3D reconstruction, meshing, and
even path planning [3]. Because of its compute-heavy nature, parallel implementations of the DT
algorithm have been the subject of much academic research. There have been works on memoryefficient
parallelizations of DT [2] as well as several approaches in implementing GPU-accelerated
versions of DT [4] [5].</p>

<h2 id="the-challenge">The Challenge</h2>

<p>There are several challenges inherent in the algorithm that make parallelization difficult:</p>
<ol>
  <li><strong>Sequential Nature of Incremental Approach:</strong> The incremental approach of the algorithm
allows vertices to be added to the existing triangulation one at a time. This sequential nature
may present contention or accuracy trade-offs when attempting to insert multiple vertices to
the existing graph at once.</li>
  <li><strong>Memory Complexity:</strong> The DT algorithm is a relatively complex algorithm with many
memory allocations and de-allocations, which presents the challenge for efficient use of
memory and improving memory locality.</li>
  <li><strong>Iteration Dependencies:</strong> As presented in [6], Incremental DT is fundamentally a randomized
incremental algorithm with iterative dependencies in which the next step may depend
on the previous step. Since the vertices are inserted in an uniform random order, there
will be a certain probability of creating a dependency with existing vertices at any given
insertion/iteration. The random nature of insertion creates problems with both cache locality
and contentions in parallelization.</li>
</ol>

<h2 id="schedule">Schedule</h2>

<p>We will aim to implement several parallel versions of the Delaunay Triangulation algorithm using
different approaches and platforms. In the case that the project progresses slowly, one of either the
GPU-accelerated implementation or distribution MPI implementation will be implemented.</p>

<table>
  <thead>
    <tr>
      <th style="text-align: center">Week</th>
      <th style="text-align: center">Dates</th>
      <th>Tasks</th>
      <th>Assignee</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center">1</td>
      <td style="text-align: center">11/4 - 11/10</td>
      <td>Understand the algorithms. Try to fix the starter code so that it compiles. Write at least two different sequential implementations. (Done!)</td>
      <td>Both</td>
    </tr>
    <tr>
      <td style="text-align: center">2</td>
      <td style="text-align: center">11/11 - 11/17</td>
      <td>Debug sequential implementations and explore GPU-accelerated implementation. (Done!)</td>
      <td>Both</td>
    </tr>
    <tr>
      <td style="text-align: center">3</td>
      <td style="text-align: center">11/18 - 11/24</td>
      <td>Complete GPU implementation without KNN <br />Implement GPU Memory Optimizations</td>
      <td>Harvey <br />Connie</td>
    </tr>
    <tr>
      <td style="text-align: center">4</td>
      <td style="text-align: center">11/25 - 12/1</td>
      <td>Debug GPU implementation and profile <br />Research distributed partition schemes <br />Implement work partitioning scheme</td>
      <td>Harvey <br />Both <br />Connie</td>
    </tr>
    <tr>
      <td style="text-align: center">5</td>
      <td style="text-align: center">12/2 - 12/8</td>
      <td>Implement efficient merging of local triangulations <br />Research/Brainstorm approaches to reducing thread contention and communication <br />Explore optimizing KNN implementation and memory optimizations</td>
      <td>Harvey <br />Both <br />Connie</td>
    </tr>
    <tr>
      <td style="text-align: center">6</td>
      <td style="text-align: center">12/9 - 12/15</td>
      <td>Profile Communication and Cache Behavior of MPI implementation <br />Profile Execution Time and Run Time Distribution <br />Complete Presentation Poster</td>
      <td>Harvey <br />Connie <br />Both</td>
    </tr>
  </tbody>
</table>

<h2 id="references">References</h2>

<ol>
  <li>Julian Shun, Guy E Blelloch, Jeremy T Fineman, Phillip B Gibbons, Aapo Kyrola, Harsha Vardhan Simhadri, and Kanat Tangwongsan. Brief announcement: the problem based benchmark suite. In <em>Proceedings of the twenty-fourth annual ACM symposium on Parallelism in algorithms and architectures</em>, pages 68–70. ACM, 2012.</li>
  <li>Daniel K Blandford, Guy E Blelloch, and Clemens Kadow. Engineering a compact parallel delaunay algorithm in 3d. In <em>Proceedings of the twenty-second annual symposium on Computational geometry</em>, pages 292–300. ACM, 2006.</li>
  <li>Pranav Kant Gaur and S. K. Bose. On recent advances in 2d constrained delaunay triangulation algorithms, 2017.</li>
  <li>Meng Qi, Thanh-Tung Cao, and Tiow-Seng Tan. Computing 2d constrained delaunay triangulation using the gpu. <em>IEEE transactions on visualization and computer graphics</em>, 19(5):736–748, 2013.</li>
  <li>Guodong Rong, Tiow-Seng Tan, Thanh-Tung Cao, et al. Computing two-dimensional delaunay triangulation using graphics hardware. In <em>Proceedings of the 2008 symposium on Interactive 3D graphics and games</em>, pages 89–97. ACM, 2008.</li>
  <li>Guy E Blelloch, Yan Gu, Julian Shun, and Yihan Sun. Parallelism in randomized incremental algorithms. In <em>Proceedings of the 28th ACM Symposium on Parallelism in Algorithms and Architectures</em>, pages 467–478. ACM, 2016.</li>
</ol>

      </section>
    </div>

    <!-- FOOTER  -->
    <div id="footer_wrap" class="outer">
      <footer class="inner">
        
        <p class="copyright">DelaunayTriangulation maintained by <a href="https://github.com/conniefan">conniefan</a></p>
        
        <p>Published with <a href="https://pages.github.com">GitHub Pages</a></p>
      </footer>
    </div>

    
  </body>
</html>
