<template>
  <div class="container">

    <div class="col_2">
      <div class="graph_1">
        <svg class="svg_g1" width="100%" height="100%" viewBox="250 250 500 500">
          <line x1="0" y1="500" x2="1000" y2="500" stroke="#666666" stroke-width="1"></line>
          <line x1="500" y1="0" x2="500" y2="1000" stroke="#666666" stroke-width="1"></line>
        </svg>
      </div>
    </div>

  </div>
</template>

<script>
import * as d3 from 'd3'

export default {
  data () {
    return {}
  },
  mounted () {

    let width = 1000;
    let height = 1000;

    let svg = d3.select('.svg_g1')

    // SVGのリセット
    svg.selectAll("*").remove()

    let g = svg.append('g')
    //console.log(g)


    // zoom
    let zoomHandler = d3.zoom()
      .on('zoom', zoomActions)

    zoomHandler(svg)

    function zoomActions () {
      g.attr('transform', d3.event.transform)
    }

    // Polygon
    function polygon(d) { 
      return "M" + d.join("L") + "Z"; 
    }

    let radius = 10;
    let angle = 0;

    var vertices = d3.range(50).map(function() {    
      let vx = 500 + radius * Math.cos(angle)
      let vy = 500 + radius * Math.sin(angle)

      angle += 5
      radius += 5

      return [vx, vy];

    });
    //console.log(vertices)

    // 領域? x座標?
    var voronoi = d3.voronoi().extent([[0, 0], [width, height]]);
    //console.log(voronoi)

    //var path = svg.append("g").selectAll("path");
    //console.log(path)

    var path = g.selectAll("path");

    path
      .data( voronoi.polygons(vertices)  )
      .enter()
      .append("path")
      .attr("stroke","white") 
      //.attr("fill", function(d,i) {return c10[i % 10]} )
      .attr("fill", "#42b951")
      .attr("d", polygon);


    // text
    g.selectAll('text')
      .data(vertices)
      .enter()
      .append('text')
      .attr('x', function (d) {
        return d[0]
      })
      .attr('y', function (d) {
        return d[1]
      })
      .text("サンプル")
      .attr('fill', '#000000')
      .attr('font-size', '14')
      .attr('font-weight', 'bold')
      .attr('text-anchor', 'middle')      
      //.attr('fill-opacity', 0)
      .on("click", function(d) {
        alert(d[0] + ", " + d[1])
      })

  }

}

</script>
