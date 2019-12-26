cola Report for GDS830
==================

**Date**: 2019-12-25 22:20:24 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 21342    50
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:skmeans](#SD-skmeans)   |          2| 1.000|           1.000|       1.000|** |           |
|[SD:pam](#SD-pam)           |          4| 1.000|           0.979|       0.990|** |2,3        |
|[SD:NMF](#SD-NMF)           |          3| 1.000|           0.974|       0.991|** |2          |
|[CV:hclust](#CV-hclust)     |          2| 1.000|           0.961|       0.982|** |           |
|[CV:pam](#CV-pam)           |          3| 1.000|           0.975|       0.990|** |2          |
|[MAD:hclust](#MAD-hclust)   |          2| 1.000|           0.970|       0.987|** |           |
|[MAD:kmeans](#MAD-kmeans)   |          3| 1.000|           0.986|       0.987|** |           |
|[MAD:pam](#MAD-pam)         |          4| 1.000|           0.994|       0.997|** |2,3        |
|[MAD:NMF](#MAD-NMF)         |          3| 1.000|           0.965|       0.986|** |2          |
|[ATC:kmeans](#ATC-kmeans)   |          3| 1.000|           0.992|       0.991|** |           |
|[ATC:skmeans](#ATC-skmeans) |          3| 1.000|           0.981|       0.992|** |2          |
|[ATC:pam](#ATC-pam)         |          3| 1.000|           1.000|       1.000|** |2          |
|[ATC:mclust](#ATC-mclust)   |          2| 1.000|           0.991|       0.995|** |           |
|[ATC:NMF](#ATC-NMF)         |          3| 1.000|           0.951|       0.981|** |           |
|[CV:kmeans](#CV-kmeans)     |          3| 0.944|           0.959|       0.961|*  |           |
|[SD:kmeans](#SD-kmeans)     |          3| 0.943|           0.956|       0.959|*  |           |
|[MAD:skmeans](#MAD-skmeans) |          3| 0.939|           0.949|       0.978|*  |2          |
|[CV:NMF](#CV-NMF)           |          4| 0.923|           0.920|       0.954|*  |2,3        |
|[ATC:hclust](#ATC-hclust)   |          5| 0.915|           0.893|       0.939|*  |2,3        |
|[SD:hclust](#SD-hclust)     |          3| 0.908|           0.910|       0.960|*  |2          |
|[CV:skmeans](#CV-skmeans)   |          3| 0.856|           0.838|       0.938|   |           |
|[CV:mclust](#CV-mclust)     |          3| 0.811|           0.856|       0.935|   |           |
|[MAD:mclust](#MAD-mclust)   |          5| 0.704|           0.795|       0.878|   |           |
|[SD:mclust](#SD-mclust)     |          3| 0.622|           0.826|       0.913|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 1.000           0.983       0.992          0.386 0.607   0.607
#&gt; CV:NMF      2 1.000           0.990       0.995          0.393 0.607   0.607
#&gt; MAD:NMF     2 1.000           0.980       0.991          0.391 0.607   0.607
#&gt; ATC:NMF     2 0.486           0.796       0.893          0.452 0.556   0.556
#&gt; SD:skmeans  2 1.000           1.000       1.000          0.471 0.530   0.530
#&gt; CV:skmeans  2 0.735           0.842       0.927          0.461 0.519   0.519
#&gt; MAD:skmeans 2 1.000           0.976       0.991          0.477 0.519   0.519
#&gt; ATC:skmeans 2 1.000           0.979       0.986          0.474 0.519   0.519
#&gt; SD:mclust   2 0.458           0.482       0.775          0.456 0.503   0.503
#&gt; CV:mclust   2 0.499           0.882       0.913          0.450 0.519   0.519
#&gt; MAD:mclust  2 0.689           0.818       0.912          0.358 0.726   0.726
#&gt; ATC:mclust  2 1.000           0.991       0.995          0.468 0.530   0.530
#&gt; SD:kmeans   2 0.426           0.768       0.829          0.356 0.650   0.650
#&gt; CV:kmeans   2 0.362           0.843       0.823          0.363 0.650   0.650
#&gt; MAD:kmeans  2 0.393           0.280       0.614          0.364 0.556   0.556
#&gt; ATC:kmeans  2 0.486           0.772       0.854          0.333 0.556   0.556
#&gt; SD:pam      2 1.000           1.000       1.000          0.351 0.650   0.650
#&gt; CV:pam      2 1.000           1.000       1.000          0.351 0.650   0.650
#&gt; MAD:pam     2 1.000           1.000       1.000          0.351 0.650   0.650
#&gt; ATC:pam     2 1.000           1.000       1.000          0.327 0.673   0.673
#&gt; SD:hclust   2 1.000           0.999       1.000          0.351 0.650   0.650
#&gt; CV:hclust   2 1.000           0.961       0.982          0.334 0.673   0.673
#&gt; MAD:hclust  2 1.000           0.970       0.987          0.344 0.650   0.650
#&gt; ATC:hclust  2 1.000           1.000       1.000          0.216 0.784   0.784
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 1.000           0.974       0.991          0.499 0.731   0.588
#&gt; CV:NMF      3 0.968           0.948       0.979          0.514 0.718   0.566
#&gt; MAD:NMF     3 1.000           0.965       0.986          0.489 0.731   0.588
#&gt; ATC:NMF     3 1.000           0.951       0.981          0.326 0.591   0.406
#&gt; SD:skmeans  3 0.825           0.913       0.961          0.354 0.746   0.557
#&gt; CV:skmeans  3 0.856           0.838       0.938          0.433 0.726   0.518
#&gt; MAD:skmeans 3 0.939           0.949       0.978          0.353 0.754   0.561
#&gt; ATC:skmeans 3 1.000           0.981       0.992          0.318 0.784   0.610
#&gt; SD:mclust   3 0.622           0.826       0.913          0.336 0.603   0.376
#&gt; CV:mclust   3 0.811           0.856       0.935          0.389 0.760   0.574
#&gt; MAD:mclust  3 0.423           0.594       0.783          0.619 0.653   0.522
#&gt; ATC:mclust  3 0.771           0.813       0.919          0.263 0.776   0.610
#&gt; SD:kmeans   3 0.943           0.956       0.959          0.492 0.764   0.651
#&gt; CV:kmeans   3 0.944           0.959       0.961          0.498 0.764   0.651
#&gt; MAD:kmeans  3 1.000           0.986       0.987          0.523 0.616   0.445
#&gt; ATC:kmeans  3 1.000           0.992       0.991          0.553 0.919   0.856
#&gt; SD:pam      3 1.000           0.999       1.000          0.575 0.798   0.688
#&gt; CV:pam      3 1.000           0.975       0.990          0.601 0.798   0.688
#&gt; MAD:pam     3 1.000           1.000       1.000          0.576 0.798   0.688
#&gt; ATC:pam     3 1.000           1.000       1.000          0.508 0.833   0.753
#&gt; SD:hclust   3 0.908           0.910       0.960          0.559 0.838   0.751
#&gt; CV:hclust   3 0.690           0.847       0.899          0.726 0.740   0.613
#&gt; MAD:hclust  3 0.835           0.864       0.944          0.583 0.838   0.751
#&gt; ATC:hclust  3 1.000           0.999       1.000          1.369 0.704   0.622
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.876           0.913       0.950         0.2138 0.882   0.726
#&gt; CV:NMF      4 0.923           0.920       0.954         0.2296 0.838   0.616
#&gt; MAD:NMF     4 0.793           0.852       0.913         0.2486 0.837   0.620
#&gt; ATC:NMF     4 0.737           0.751       0.841         0.1698 0.864   0.692
#&gt; SD:skmeans  4 0.802           0.843       0.892         0.1763 0.789   0.484
#&gt; CV:skmeans  4 0.757           0.746       0.878         0.1360 0.776   0.451
#&gt; MAD:skmeans 4 0.798           0.741       0.898         0.1612 0.797   0.494
#&gt; ATC:skmeans 4 0.738           0.591       0.814         0.1768 0.932   0.820
#&gt; SD:mclust   4 0.784           0.887       0.920         0.0839 0.864   0.691
#&gt; CV:mclust   4 0.773           0.807       0.898         0.0629 0.909   0.776
#&gt; MAD:mclust  4 0.607           0.748       0.862         0.1984 0.716   0.396
#&gt; ATC:mclust  4 0.661           0.690       0.854         0.0894 0.848   0.664
#&gt; SD:kmeans   4 0.708           0.767       0.871         0.2925 0.909   0.803
#&gt; CV:kmeans   4 0.716           0.790       0.866         0.2598 0.909   0.803
#&gt; MAD:kmeans  4 0.700           0.712       0.808         0.2712 0.804   0.560
#&gt; ATC:kmeans  4 0.645           0.543       0.742         0.2998 0.778   0.542
#&gt; SD:pam      4 1.000           0.979       0.990         0.1658 0.912   0.803
#&gt; CV:pam      4 0.876           0.965       0.974         0.1616 0.912   0.803
#&gt; MAD:pam     4 1.000           0.994       0.997         0.1284 0.931   0.847
#&gt; ATC:pam     4 0.804           0.922       0.954         0.1605 0.948   0.897
#&gt; SD:hclust   4 0.765           0.844       0.902         0.2747 0.812   0.615
#&gt; CV:hclust   4 0.728           0.852       0.936         0.1630 0.890   0.739
#&gt; MAD:hclust  4 0.771           0.826       0.909         0.2694 0.800   0.594
#&gt; ATC:hclust  4 0.881           0.859       0.943         0.2575 0.905   0.806
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.747           0.721       0.849         0.1135 0.900   0.686
#&gt; CV:NMF      5 0.767           0.650       0.840         0.0614 0.904   0.664
#&gt; MAD:NMF     5 0.765           0.743       0.861         0.0860 0.929   0.743
#&gt; ATC:NMF     5 0.785           0.890       0.923         0.0940 0.897   0.690
#&gt; SD:skmeans  5 0.846           0.815       0.904         0.0711 0.949   0.797
#&gt; CV:skmeans  5 0.802           0.782       0.886         0.0700 0.933   0.738
#&gt; MAD:skmeans 5 0.824           0.741       0.879         0.0727 0.872   0.550
#&gt; ATC:skmeans 5 0.794           0.529       0.744         0.0757 0.864   0.589
#&gt; SD:mclust   5 0.664           0.702       0.838         0.1410 0.819   0.537
#&gt; CV:mclust   5 0.745           0.799       0.885         0.1234 0.845   0.591
#&gt; MAD:mclust  5 0.704           0.795       0.878         0.0964 0.935   0.777
#&gt; ATC:mclust  5 0.695           0.677       0.849         0.1334 0.883   0.686
#&gt; SD:kmeans   5 0.698           0.780       0.848         0.1082 0.853   0.606
#&gt; CV:kmeans   5 0.772           0.804       0.883         0.1189 0.853   0.606
#&gt; MAD:kmeans  5 0.754           0.735       0.860         0.0996 0.951   0.814
#&gt; ATC:kmeans  5 0.632           0.774       0.803         0.1125 0.795   0.420
#&gt; SD:pam      5 0.780           0.743       0.890         0.1341 0.925   0.791
#&gt; CV:pam      5 0.764           0.734       0.879         0.1246 0.910   0.750
#&gt; MAD:pam     5 0.851           0.872       0.943         0.1083 0.939   0.838
#&gt; ATC:pam     5 0.831           0.948       0.972         0.2292 0.843   0.655
#&gt; SD:hclust   5 0.831           0.875       0.923         0.0426 0.980   0.935
#&gt; CV:hclust   5 0.778           0.786       0.869         0.1027 0.971   0.908
#&gt; MAD:hclust  5 0.808           0.885       0.910         0.0567 0.975   0.916
#&gt; ATC:hclust  5 0.915           0.893       0.939         0.0485 0.913   0.780
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.716           0.567       0.758         0.0521 0.885   0.554
#&gt; CV:NMF      6 0.742           0.618       0.784         0.0592 0.892   0.554
#&gt; MAD:NMF     6 0.746           0.521       0.747         0.0449 0.949   0.768
#&gt; ATC:NMF     6 0.761           0.741       0.858         0.0315 0.987   0.943
#&gt; SD:skmeans  6 0.841           0.753       0.808         0.0393 0.980   0.903
#&gt; CV:skmeans  6 0.818           0.705       0.807         0.0389 0.968   0.842
#&gt; MAD:skmeans 6 0.824           0.639       0.824         0.0387 0.928   0.668
#&gt; ATC:skmeans 6 0.878           0.849       0.915         0.0483 0.924   0.674
#&gt; SD:mclust   6 0.695           0.567       0.740         0.0526 0.855   0.470
#&gt; CV:mclust   6 0.762           0.815       0.860         0.0766 0.894   0.605
#&gt; MAD:mclust  6 0.693           0.664       0.806         0.0653 0.927   0.696
#&gt; ATC:mclust  6 0.862           0.819       0.895         0.0592 0.916   0.715
#&gt; SD:kmeans   6 0.773           0.661       0.825         0.0633 0.974   0.884
#&gt; CV:kmeans   6 0.793           0.717       0.819         0.0629 0.939   0.756
#&gt; MAD:kmeans  6 0.797           0.644       0.813         0.0522 0.943   0.759
#&gt; ATC:kmeans  6 0.726           0.801       0.870         0.0678 0.958   0.826
#&gt; SD:pam      6 0.797           0.800       0.863         0.0758 0.930   0.760
#&gt; CV:pam      6 0.805           0.712       0.842         0.0679 0.913   0.706
#&gt; MAD:pam     6 0.896           0.837       0.938         0.0740 0.910   0.736
#&gt; ATC:pam     6 0.811           0.942       0.965         0.0123 0.993   0.978
#&gt; SD:hclust   6 0.787           0.811       0.889         0.0424 0.996   0.985
#&gt; CV:hclust   6 0.763           0.712       0.838         0.0390 0.969   0.896
#&gt; MAD:hclust  6 0.785           0.809       0.873         0.0485 0.996   0.985
#&gt; ATC:hclust  6 0.848           0.821       0.916         0.0826 0.946   0.828
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      50     0.394 2
#&gt; CV:NMF      50     0.394 2
#&gt; MAD:NMF     50     0.394 2
#&gt; ATC:NMF     47     0.391 2
#&gt; SD:skmeans  50     0.394 2
#&gt; CV:skmeans  48     0.392 2
#&gt; MAD:skmeans 49     0.393 2
#&gt; ATC:skmeans 50     0.394 2
#&gt; SD:mclust   30     0.414 2
#&gt; CV:mclust   50     0.394 2
#&gt; MAD:mclust  48     0.392 2
#&gt; ATC:mclust  50     0.394 2
#&gt; SD:kmeans   44     0.387 2
#&gt; CV:kmeans   50     0.394 2
#&gt; MAD:kmeans   0        NA 2
#&gt; ATC:kmeans  41     0.383 2
#&gt; SD:pam      50     0.394 2
#&gt; CV:pam      50     0.394 2
#&gt; MAD:pam     50     0.394 2
#&gt; ATC:pam     50     0.394 2
#&gt; SD:hclust   50     0.394 2
#&gt; CV:hclust   49     0.393 2
#&gt; MAD:hclust  50     0.394 2
#&gt; ATC:hclust  50     0.394 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      49     0.368 3
#&gt; CV:NMF      48     0.423 3
#&gt; MAD:NMF     49     0.368 3
#&gt; ATC:NMF     49     0.368 3
#&gt; SD:skmeans  49     0.449 3
#&gt; CV:skmeans  45     0.447 3
#&gt; MAD:skmeans 49     0.449 3
#&gt; ATC:skmeans 50     0.370 3
#&gt; SD:mclust   47     0.440 3
#&gt; CV:mclust   47     0.437 3
#&gt; MAD:mclust  37     0.413 3
#&gt; ATC:mclust  45     0.421 3
#&gt; SD:kmeans   49     0.368 3
#&gt; CV:kmeans   50     0.370 3
#&gt; MAD:kmeans  50     0.370 3
#&gt; ATC:kmeans  50     0.370 3
#&gt; SD:pam      50     0.370 3
#&gt; CV:pam      49     0.368 3
#&gt; MAD:pam     50     0.370 3
#&gt; ATC:pam     50     0.370 3
#&gt; SD:hclust   47     0.366 3
#&gt; CV:hclust   48     0.367 3
#&gt; MAD:hclust  46     0.364 3
#&gt; ATC:hclust  50     0.370 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      49     0.464 4
#&gt; CV:NMF      49     0.436 4
#&gt; MAD:NMF     48     0.430 4
#&gt; ATC:NMF     46     0.412 4
#&gt; SD:skmeans  49     0.348 4
#&gt; CV:skmeans  40     0.406 4
#&gt; MAD:skmeans 40     0.406 4
#&gt; ATC:skmeans 33     0.397 4
#&gt; SD:mclust   50     0.511 4
#&gt; CV:mclust   47     0.504 4
#&gt; MAD:mclust  44     0.411 4
#&gt; ATC:mclust  37     0.413 4
#&gt; SD:kmeans   41     0.407 4
#&gt; CV:kmeans   49     0.509 4
#&gt; MAD:kmeans  39     0.405 4
#&gt; ATC:kmeans  33     0.316 4
#&gt; SD:pam      50     0.512 4
#&gt; CV:pam      50     0.512 4
#&gt; MAD:pam     50     0.560 4
#&gt; ATC:pam     50     0.349 4
#&gt; SD:hclust   49     0.432 4
#&gt; CV:hclust   48     0.509 4
#&gt; MAD:hclust  48     0.437 4
#&gt; ATC:hclust  44     0.339 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      43     0.436 5
#&gt; CV:NMF      38     0.486 5
#&gt; MAD:NMF     42     0.422 5
#&gt; ATC:NMF     50     0.479 5
#&gt; SD:skmeans  45     0.471 5
#&gt; CV:skmeans  43     0.463 5
#&gt; MAD:skmeans 42     0.459 5
#&gt; ATC:skmeans 28     0.400 5
#&gt; SD:mclust   42     0.494 5
#&gt; CV:mclust   47     0.512 5
#&gt; MAD:mclust  50     0.536 5
#&gt; ATC:mclust  39     0.521 5
#&gt; SD:kmeans   45     0.483 5
#&gt; CV:kmeans   45     0.483 5
#&gt; MAD:kmeans  41     0.517 5
#&gt; ATC:kmeans  47     0.404 5
#&gt; SD:pam      43     0.451 5
#&gt; CV:pam      42     0.457 5
#&gt; MAD:pam     47     0.503 5
#&gt; ATC:pam     50     0.331 5
#&gt; SD:hclust   50     0.473 5
#&gt; CV:hclust   47     0.464 5
#&gt; MAD:hclust  50     0.473 5
#&gt; ATC:hclust  47     0.326 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      30     0.445 6
#&gt; CV:NMF      35     0.418 6
#&gt; MAD:NMF     31     0.474 6
#&gt; ATC:NMF     43     0.493 6
#&gt; SD:skmeans  44     0.440 6
#&gt; CV:skmeans  35     0.435 6
#&gt; MAD:skmeans 36     0.455 6
#&gt; ATC:skmeans 48     0.399 6
#&gt; SD:mclust   34     0.461 6
#&gt; CV:mclust   46     0.477 6
#&gt; MAD:mclust  36     0.472 6
#&gt; ATC:mclust  44     0.495 6
#&gt; SD:kmeans   38     0.472 6
#&gt; CV:kmeans   43     0.487 6
#&gt; MAD:kmeans  38     0.509 6
#&gt; ATC:kmeans  46     0.474 6
#&gt; SD:pam      49     0.439 6
#&gt; CV:pam      41     0.448 6
#&gt; MAD:pam     46     0.465 6
#&gt; ATC:pam     50     0.315 6
#&gt; SD:hclust   48     0.466 6
#&gt; CV:hclust   42     0.448 6
#&gt; MAD:hclust  47     0.463 6
#&gt; ATC:hclust  44     0.401 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.999       1.000         0.3512 0.650   0.650
#> 3 3 0.908           0.910       0.960         0.5593 0.838   0.751
#> 4 4 0.765           0.844       0.902         0.2747 0.812   0.615
#> 5 5 0.831           0.875       0.923         0.0426 0.980   0.935
#> 6 6 0.787           0.811       0.889         0.0424 0.996   0.985
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1   0.000      1.000 1.000 0.000
#&gt; GSM28736     1   0.000      1.000 1.000 0.000
#&gt; GSM28737     1   0.000      1.000 1.000 0.000
#&gt; GSM11249     1   0.000      1.000 1.000 0.000
#&gt; GSM28745     2   0.000      1.000 0.000 1.000
#&gt; GSM11244     2   0.000      1.000 0.000 1.000
#&gt; GSM28748     2   0.000      1.000 0.000 1.000
#&gt; GSM11266     2   0.000      1.000 0.000 1.000
#&gt; GSM28730     2   0.000      1.000 0.000 1.000
#&gt; GSM11253     2   0.000      1.000 0.000 1.000
#&gt; GSM11254     2   0.000      1.000 0.000 1.000
#&gt; GSM11260     2   0.000      1.000 0.000 1.000
#&gt; GSM28733     2   0.000      1.000 0.000 1.000
#&gt; GSM11265     1   0.000      1.000 1.000 0.000
#&gt; GSM28739     1   0.000      1.000 1.000 0.000
#&gt; GSM11243     1   0.000      1.000 1.000 0.000
#&gt; GSM28740     1   0.000      1.000 1.000 0.000
#&gt; GSM11259     1   0.000      1.000 1.000 0.000
#&gt; GSM28726     1   0.000      1.000 1.000 0.000
#&gt; GSM28743     1   0.000      1.000 1.000 0.000
#&gt; GSM11256     1   0.000      1.000 1.000 0.000
#&gt; GSM11262     1   0.000      1.000 1.000 0.000
#&gt; GSM28724     1   0.000      1.000 1.000 0.000
#&gt; GSM28725     1   0.000      1.000 1.000 0.000
#&gt; GSM11263     1   0.000      1.000 1.000 0.000
#&gt; GSM11267     1   0.000      1.000 1.000 0.000
#&gt; GSM28744     1   0.000      1.000 1.000 0.000
#&gt; GSM28734     1   0.000      1.000 1.000 0.000
#&gt; GSM28747     1   0.000      1.000 1.000 0.000
#&gt; GSM11257     1   0.000      1.000 1.000 0.000
#&gt; GSM11252     1   0.000      1.000 1.000 0.000
#&gt; GSM11264     1   0.000      1.000 1.000 0.000
#&gt; GSM11247     1   0.000      1.000 1.000 0.000
#&gt; GSM11258     1   0.000      1.000 1.000 0.000
#&gt; GSM28728     1   0.000      1.000 1.000 0.000
#&gt; GSM28746     1   0.000      1.000 1.000 0.000
#&gt; GSM28738     1   0.000      1.000 1.000 0.000
#&gt; GSM28741     2   0.000      1.000 0.000 1.000
#&gt; GSM28729     1   0.000      1.000 1.000 0.000
#&gt; GSM28742     1   0.000      1.000 1.000 0.000
#&gt; GSM11250     2   0.000      1.000 0.000 1.000
#&gt; GSM11245     1   0.000      1.000 1.000 0.000
#&gt; GSM11246     1   0.000      1.000 1.000 0.000
#&gt; GSM11261     1   0.118      0.984 0.984 0.016
#&gt; GSM11248     1   0.000      1.000 1.000 0.000
#&gt; GSM28732     1   0.000      1.000 1.000 0.000
#&gt; GSM11255     1   0.000      1.000 1.000 0.000
#&gt; GSM28731     1   0.000      1.000 1.000 0.000
#&gt; GSM28727     1   0.000      1.000 1.000 0.000
#&gt; GSM11251     1   0.000      1.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28736     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28737     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11249     1   0.611      0.440 0.604 0.000 0.396
#&gt; GSM28745     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11244     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28748     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11266     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28730     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11253     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11254     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11260     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28733     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11265     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28739     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11243     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28740     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11259     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28726     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28743     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11256     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11262     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28724     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28725     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11263     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11267     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28744     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28734     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28747     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11257     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11252     1   0.518      0.682 0.744 0.000 0.256
#&gt; GSM11264     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11247     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11258     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28728     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28746     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28738     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28741     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM28729     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28742     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11250     2   0.000      1.000 0.000 1.000 0.000
#&gt; GSM11245     1   0.518      0.682 0.744 0.000 0.256
#&gt; GSM11246     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11261     1   0.688      0.330 0.556 0.016 0.428
#&gt; GSM11248     1   0.611      0.440 0.604 0.000 0.396
#&gt; GSM28732     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11255     1   0.506      0.698 0.756 0.000 0.244
#&gt; GSM28731     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM28727     1   0.000      0.935 1.000 0.000 0.000
#&gt; GSM11251     1   0.000      0.935 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM28736     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM28737     1  0.4103      0.749 0.744 0.000 0.000 0.256
#&gt; GSM11249     4  0.4843      0.576 0.000 0.000 0.396 0.604
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.4103      0.749 0.744 0.000 0.000 0.256
#&gt; GSM28739     1  0.4103      0.749 0.744 0.000 0.000 0.256
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28740     1  0.4103      0.749 0.744 0.000 0.000 0.256
#&gt; GSM11259     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM28726     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM28743     1  0.4103      0.749 0.744 0.000 0.000 0.256
#&gt; GSM11256     4  0.0000      0.683 0.000 0.000 0.000 1.000
#&gt; GSM11262     1  0.4103      0.749 0.744 0.000 0.000 0.256
#&gt; GSM28724     1  0.0469      0.874 0.988 0.000 0.000 0.012
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.0000      0.683 0.000 0.000 0.000 1.000
#&gt; GSM28734     4  0.0000      0.683 0.000 0.000 0.000 1.000
#&gt; GSM28747     1  0.0469      0.874 0.988 0.000 0.000 0.012
#&gt; GSM11257     1  0.0336      0.874 0.992 0.000 0.000 0.008
#&gt; GSM11252     4  0.6698      0.649 0.140 0.000 0.256 0.604
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11258     4  0.0000      0.683 0.000 0.000 0.000 1.000
#&gt; GSM28728     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM28746     1  0.4304      0.699 0.716 0.000 0.000 0.284
#&gt; GSM28738     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28729     1  0.1792      0.859 0.932 0.000 0.000 0.068
#&gt; GSM28742     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11245     4  0.6698      0.649 0.140 0.000 0.256 0.604
#&gt; GSM11246     1  0.4103      0.749 0.744 0.000 0.000 0.256
#&gt; GSM11261     4  0.6165      0.521 0.024 0.016 0.428 0.532
#&gt; GSM11248     4  0.4843      0.576 0.000 0.000 0.396 0.604
#&gt; GSM28732     1  0.0817      0.872 0.976 0.000 0.000 0.024
#&gt; GSM11255     4  0.7694      0.481 0.308 0.000 0.244 0.448
#&gt; GSM28731     1  0.1940      0.856 0.924 0.000 0.000 0.076
#&gt; GSM28727     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.874 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3   p4    p5
#&gt; GSM28735     1  0.0000      0.862 1.000 0.000 0.000 0.00 0.000
#&gt; GSM28736     1  0.0000      0.862 1.000 0.000 0.000 0.00 0.000
#&gt; GSM28737     1  0.3661      0.732 0.724 0.000 0.000 0.00 0.276
#&gt; GSM11249     5  0.3106      0.763 0.000 0.000 0.140 0.02 0.840
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM11265     1  0.3661      0.732 0.724 0.000 0.000 0.00 0.276
#&gt; GSM28739     1  0.3661      0.732 0.724 0.000 0.000 0.00 0.276
#&gt; GSM11243     3  0.2471      0.891 0.000 0.000 0.864 0.00 0.136
#&gt; GSM28740     1  0.3661      0.732 0.724 0.000 0.000 0.00 0.276
#&gt; GSM11259     1  0.0000      0.862 1.000 0.000 0.000 0.00 0.000
#&gt; GSM28726     1  0.0000      0.862 1.000 0.000 0.000 0.00 0.000
#&gt; GSM28743     1  0.3661      0.732 0.724 0.000 0.000 0.00 0.276
#&gt; GSM11256     4  0.0000      1.000 0.000 0.000 0.000 1.00 0.000
#&gt; GSM11262     1  0.3661      0.732 0.724 0.000 0.000 0.00 0.276
#&gt; GSM28724     1  0.0404      0.863 0.988 0.000 0.000 0.00 0.012
#&gt; GSM28725     3  0.0000      0.947 0.000 0.000 1.000 0.00 0.000
#&gt; GSM11263     3  0.0000      0.947 0.000 0.000 1.000 0.00 0.000
#&gt; GSM11267     3  0.0000      0.947 0.000 0.000 1.000 0.00 0.000
#&gt; GSM28744     4  0.0000      1.000 0.000 0.000 0.000 1.00 0.000
#&gt; GSM28734     4  0.0000      1.000 0.000 0.000 0.000 1.00 0.000
#&gt; GSM28747     1  0.0404      0.864 0.988 0.000 0.000 0.00 0.012
#&gt; GSM11257     1  0.0794      0.862 0.972 0.000 0.000 0.00 0.028
#&gt; GSM11252     5  0.3106      0.805 0.140 0.000 0.000 0.02 0.840
#&gt; GSM11264     3  0.0000      0.947 0.000 0.000 1.000 0.00 0.000
#&gt; GSM11247     3  0.2471      0.891 0.000 0.000 0.864 0.00 0.136
#&gt; GSM11258     4  0.0000      1.000 0.000 0.000 0.000 1.00 0.000
#&gt; GSM28728     1  0.0000      0.862 1.000 0.000 0.000 0.00 0.000
#&gt; GSM28746     1  0.4157      0.697 0.716 0.000 0.000 0.02 0.264
#&gt; GSM28738     1  0.0609      0.862 0.980 0.000 0.000 0.00 0.020
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM28729     1  0.1851      0.845 0.912 0.000 0.000 0.00 0.088
#&gt; GSM28742     1  0.0000      0.862 1.000 0.000 0.000 0.00 0.000
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000 0.000 0.00 0.000
#&gt; GSM11245     5  0.3106      0.805 0.140 0.000 0.000 0.02 0.840
#&gt; GSM11246     1  0.3661      0.732 0.724 0.000 0.000 0.00 0.276
#&gt; GSM11261     5  0.2815      0.729 0.024 0.012 0.044 0.02 0.900
#&gt; GSM11248     5  0.3106      0.763 0.000 0.000 0.140 0.02 0.840
#&gt; GSM28732     1  0.0703      0.862 0.976 0.000 0.000 0.00 0.024
#&gt; GSM11255     5  0.3730      0.613 0.288 0.000 0.000 0.00 0.712
#&gt; GSM28731     1  0.1965      0.842 0.904 0.000 0.000 0.00 0.096
#&gt; GSM28727     1  0.0000      0.862 1.000 0.000 0.000 0.00 0.000
#&gt; GSM11251     1  0.0000      0.862 1.000 0.000 0.000 0.00 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3 p4    p5    p6
#&gt; GSM28735     1  0.0291      0.802 0.992  0 0.000  0 0.004 0.004
#&gt; GSM28736     1  0.0291      0.802 0.992  0 0.000  0 0.004 0.004
#&gt; GSM28737     1  0.3717      0.687 0.616  0 0.000  0 0.384 0.000
#&gt; GSM11249     6  0.2219      0.600 0.000  0 0.136  0 0.000 0.864
#&gt; GSM28745     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM11265     1  0.3717      0.687 0.616  0 0.000  0 0.384 0.000
#&gt; GSM28739     1  0.3717      0.687 0.616  0 0.000  0 0.384 0.000
#&gt; GSM11243     3  0.2814      0.822 0.000  0 0.820  0 0.172 0.008
#&gt; GSM28740     1  0.3717      0.687 0.616  0 0.000  0 0.384 0.000
#&gt; GSM11259     1  0.0865      0.811 0.964  0 0.000  0 0.036 0.000
#&gt; GSM28726     1  0.0291      0.802 0.992  0 0.000  0 0.004 0.004
#&gt; GSM28743     1  0.3717      0.687 0.616  0 0.000  0 0.384 0.000
#&gt; GSM11256     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; GSM11262     1  0.3717      0.687 0.616  0 0.000  0 0.384 0.000
#&gt; GSM28724     1  0.0914      0.806 0.968  0 0.000  0 0.016 0.016
#&gt; GSM28725     3  0.0000      0.919 0.000  0 1.000  0 0.000 0.000
#&gt; GSM11263     3  0.0000      0.919 0.000  0 1.000  0 0.000 0.000
#&gt; GSM11267     3  0.0000      0.919 0.000  0 1.000  0 0.000 0.000
#&gt; GSM28744     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; GSM28734     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; GSM28747     1  0.0937      0.812 0.960  0 0.000  0 0.040 0.000
#&gt; GSM11257     1  0.3435      0.675 0.804  0 0.000  0 0.060 0.136
#&gt; GSM11252     6  0.2260      0.682 0.140  0 0.000  0 0.000 0.860
#&gt; GSM11264     3  0.0000      0.919 0.000  0 1.000  0 0.000 0.000
#&gt; GSM11247     3  0.2814      0.822 0.000  0 0.820  0 0.172 0.008
#&gt; GSM11258     4  0.0000      1.000 0.000  0 0.000  1 0.000 0.000
#&gt; GSM28728     1  0.0603      0.806 0.980  0 0.000  0 0.016 0.004
#&gt; GSM28746     1  0.4890      0.660 0.660  0 0.000  0 0.180 0.160
#&gt; GSM28738     1  0.3354      0.678 0.812  0 0.000  0 0.060 0.128
#&gt; GSM28741     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM28729     1  0.2558      0.790 0.840  0 0.000  0 0.156 0.004
#&gt; GSM28742     1  0.0291      0.802 0.992  0 0.000  0 0.004 0.004
#&gt; GSM11250     2  0.0000      1.000 0.000  1 0.000  0 0.000 0.000
#&gt; GSM11245     6  0.2260      0.682 0.140  0 0.000  0 0.000 0.860
#&gt; GSM11246     1  0.3717      0.687 0.616  0 0.000  0 0.384 0.000
#&gt; GSM11261     5  0.2823      0.000 0.000  0 0.000  0 0.796 0.204
#&gt; GSM11248     6  0.2219      0.600 0.000  0 0.136  0 0.000 0.864
#&gt; GSM28732     1  0.1501      0.810 0.924  0 0.000  0 0.076 0.000
#&gt; GSM11255     6  0.5134      0.388 0.228  0 0.000  0 0.152 0.620
#&gt; GSM28731     1  0.2778      0.786 0.824  0 0.000  0 0.168 0.008
#&gt; GSM28727     1  0.0713      0.810 0.972  0 0.000  0 0.028 0.000
#&gt; GSM11251     1  0.0713      0.810 0.972  0 0.000  0 0.028 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:hclust 50     0.394 2
#> SD:hclust 47     0.366 3
#> SD:hclust 49     0.432 4
#> SD:hclust 50     0.473 5
#> SD:hclust 48     0.466 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:kmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.426           0.768       0.829         0.3563 0.650   0.650
#> 3 3 0.943           0.956       0.959         0.4921 0.764   0.651
#> 4 4 0.708           0.767       0.871         0.2925 0.909   0.803
#> 5 5 0.698           0.780       0.848         0.1082 0.853   0.606
#> 6 6 0.773           0.661       0.825         0.0633 0.974   0.884
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1   0.939      0.820 0.644 0.356
#&gt; GSM28736     1   0.939      0.820 0.644 0.356
#&gt; GSM28737     1   0.939      0.820 0.644 0.356
#&gt; GSM11249     1   0.000      0.520 1.000 0.000
#&gt; GSM28745     2   0.000      0.975 0.000 1.000
#&gt; GSM11244     2   0.000      0.975 0.000 1.000
#&gt; GSM28748     2   0.000      0.975 0.000 1.000
#&gt; GSM11266     2   0.000      0.975 0.000 1.000
#&gt; GSM28730     2   0.000      0.975 0.000 1.000
#&gt; GSM11253     2   0.000      0.975 0.000 1.000
#&gt; GSM11254     2   0.000      0.975 0.000 1.000
#&gt; GSM11260     2   0.000      0.975 0.000 1.000
#&gt; GSM28733     2   0.000      0.975 0.000 1.000
#&gt; GSM11265     1   0.939      0.820 0.644 0.356
#&gt; GSM28739     1   0.939      0.820 0.644 0.356
#&gt; GSM11243     1   0.680      0.308 0.820 0.180
#&gt; GSM28740     1   0.939      0.820 0.644 0.356
#&gt; GSM11259     1   0.939      0.820 0.644 0.356
#&gt; GSM28726     1   0.939      0.820 0.644 0.356
#&gt; GSM28743     1   0.939      0.820 0.644 0.356
#&gt; GSM11256     1   0.855      0.793 0.720 0.280
#&gt; GSM11262     1   0.939      0.820 0.644 0.356
#&gt; GSM28724     1   0.939      0.820 0.644 0.356
#&gt; GSM28725     1   0.680      0.308 0.820 0.180
#&gt; GSM11263     1   0.680      0.308 0.820 0.180
#&gt; GSM11267     1   0.680      0.308 0.820 0.180
#&gt; GSM28744     1   0.855      0.793 0.720 0.280
#&gt; GSM28734     1   0.855      0.793 0.720 0.280
#&gt; GSM28747     1   0.939      0.820 0.644 0.356
#&gt; GSM11257     1   0.917      0.813 0.668 0.332
#&gt; GSM11252     1   0.866      0.798 0.712 0.288
#&gt; GSM11264     1   0.680      0.308 0.820 0.180
#&gt; GSM11247     1   0.680      0.308 0.820 0.180
#&gt; GSM11258     1   0.855      0.793 0.720 0.280
#&gt; GSM28728     1   0.939      0.820 0.644 0.356
#&gt; GSM28746     1   0.881      0.802 0.700 0.300
#&gt; GSM28738     1   0.939      0.820 0.644 0.356
#&gt; GSM28741     2   0.605      0.703 0.148 0.852
#&gt; GSM28729     1   0.939      0.820 0.644 0.356
#&gt; GSM28742     1   0.939      0.820 0.644 0.356
#&gt; GSM11250     2   0.000      0.975 0.000 1.000
#&gt; GSM11245     1   0.866      0.798 0.712 0.288
#&gt; GSM11246     1   0.939      0.820 0.644 0.356
#&gt; GSM11261     1   0.971      0.655 0.600 0.400
#&gt; GSM11248     1   0.000      0.520 1.000 0.000
#&gt; GSM28732     1   0.939      0.820 0.644 0.356
#&gt; GSM11255     1   0.866      0.798 0.712 0.288
#&gt; GSM28731     1   0.939      0.820 0.644 0.356
#&gt; GSM28727     1   0.939      0.820 0.644 0.356
#&gt; GSM11251     1   0.939      0.820 0.644 0.356
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28736     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28737     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM11249     3  0.2261      0.994 0.068 0.000 0.932
#&gt; GSM28745     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM11244     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM28748     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM11266     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM28730     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM11253     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM11254     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM11260     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM28733     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM11265     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28739     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM11243     3  0.2749      0.994 0.064 0.012 0.924
#&gt; GSM28740     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM11259     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28726     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28743     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM11256     1  0.3875      0.886 0.888 0.044 0.068
#&gt; GSM11262     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28724     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28725     3  0.2400      0.997 0.064 0.004 0.932
#&gt; GSM11263     3  0.2400      0.997 0.064 0.004 0.932
#&gt; GSM11267     3  0.2400      0.997 0.064 0.004 0.932
#&gt; GSM28744     1  0.3875      0.886 0.888 0.044 0.068
#&gt; GSM28734     1  0.3780      0.887 0.892 0.044 0.064
#&gt; GSM28747     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11257     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM11252     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11264     3  0.2400      0.997 0.064 0.004 0.932
#&gt; GSM11247     3  0.2749      0.994 0.064 0.012 0.924
#&gt; GSM11258     1  0.3780      0.887 0.892 0.044 0.064
#&gt; GSM28728     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28746     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28738     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28741     1  0.6398      0.234 0.580 0.416 0.004
#&gt; GSM28729     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28742     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM11250     2  0.1860      1.000 0.052 0.948 0.000
#&gt; GSM11245     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11246     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM11261     1  0.1999      0.929 0.952 0.012 0.036
#&gt; GSM11248     3  0.2261      0.994 0.068 0.000 0.932
#&gt; GSM28732     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM11255     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28731     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.967 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.0817      0.723 0.976 0.000 0.000 0.024
#&gt; GSM28736     1  0.1302      0.715 0.956 0.000 0.000 0.044
#&gt; GSM28737     1  0.4356      0.627 0.708 0.000 0.000 0.292
#&gt; GSM11249     3  0.1940      0.927 0.000 0.000 0.924 0.076
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.4972      0.444 0.544 0.000 0.000 0.456
#&gt; GSM28739     1  0.4972      0.444 0.544 0.000 0.000 0.456
#&gt; GSM11243     3  0.1474      0.950 0.000 0.000 0.948 0.052
#&gt; GSM28740     1  0.4972      0.444 0.544 0.000 0.000 0.456
#&gt; GSM11259     1  0.0000      0.728 1.000 0.000 0.000 0.000
#&gt; GSM28726     1  0.1302      0.715 0.956 0.000 0.000 0.044
#&gt; GSM28743     1  0.4972      0.444 0.544 0.000 0.000 0.456
#&gt; GSM11256     4  0.3123      0.916 0.156 0.000 0.000 0.844
#&gt; GSM11262     1  0.4972      0.444 0.544 0.000 0.000 0.456
#&gt; GSM28724     1  0.0707      0.731 0.980 0.000 0.000 0.020
#&gt; GSM28725     3  0.0000      0.966 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      0.966 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      0.966 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.3123      0.916 0.156 0.000 0.000 0.844
#&gt; GSM28734     4  0.2469      0.911 0.108 0.000 0.000 0.892
#&gt; GSM28747     1  0.2345      0.721 0.900 0.000 0.000 0.100
#&gt; GSM11257     1  0.3172      0.667 0.840 0.000 0.000 0.160
#&gt; GSM11252     1  0.4830      0.501 0.608 0.000 0.000 0.392
#&gt; GSM11264     3  0.0000      0.966 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.1474      0.950 0.000 0.000 0.948 0.052
#&gt; GSM11258     4  0.1637      0.857 0.060 0.000 0.000 0.940
#&gt; GSM28728     1  0.0188      0.728 0.996 0.000 0.000 0.004
#&gt; GSM28746     1  0.4164      0.648 0.736 0.000 0.000 0.264
#&gt; GSM28738     1  0.1302      0.715 0.956 0.000 0.000 0.044
#&gt; GSM28741     1  0.4343      0.439 0.732 0.264 0.000 0.004
#&gt; GSM28729     1  0.1211      0.717 0.960 0.000 0.000 0.040
#&gt; GSM28742     1  0.1302      0.715 0.956 0.000 0.000 0.044
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11245     1  0.4877      0.480 0.592 0.000 0.000 0.408
#&gt; GSM11246     1  0.4955      0.463 0.556 0.000 0.000 0.444
#&gt; GSM11261     1  0.6194      0.436 0.644 0.000 0.096 0.260
#&gt; GSM11248     3  0.1940      0.927 0.000 0.000 0.924 0.076
#&gt; GSM28732     1  0.0188      0.729 0.996 0.000 0.000 0.004
#&gt; GSM11255     1  0.4855      0.517 0.600 0.000 0.000 0.400
#&gt; GSM28731     1  0.1557      0.729 0.944 0.000 0.000 0.056
#&gt; GSM28727     1  0.1557      0.729 0.944 0.000 0.000 0.056
#&gt; GSM11251     1  0.1557      0.729 0.944 0.000 0.000 0.056
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.0798      0.773 0.008 0.000 0.000 0.016 0.976
#&gt; GSM28736     5  0.0798      0.773 0.008 0.000 0.000 0.016 0.976
#&gt; GSM28737     1  0.3752      0.744 0.708 0.000 0.000 0.000 0.292
#&gt; GSM11249     3  0.4035      0.753 0.060 0.000 0.784 0.156 0.000
#&gt; GSM28745     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3427      0.862 0.796 0.000 0.000 0.012 0.192
#&gt; GSM28739     1  0.3427      0.862 0.796 0.000 0.000 0.012 0.192
#&gt; GSM11243     3  0.3704      0.815 0.092 0.000 0.820 0.088 0.000
#&gt; GSM28740     1  0.3427      0.862 0.796 0.000 0.000 0.012 0.192
#&gt; GSM11259     5  0.1124      0.771 0.036 0.000 0.000 0.004 0.960
#&gt; GSM28726     5  0.0798      0.773 0.008 0.000 0.000 0.016 0.976
#&gt; GSM28743     1  0.3427      0.862 0.796 0.000 0.000 0.012 0.192
#&gt; GSM11256     4  0.3193      0.953 0.132 0.000 0.000 0.840 0.028
#&gt; GSM11262     1  0.3427      0.862 0.796 0.000 0.000 0.012 0.192
#&gt; GSM28724     5  0.1965      0.763 0.052 0.000 0.000 0.024 0.924
#&gt; GSM28725     3  0.0000      0.885 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.885 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.885 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     4  0.3193      0.953 0.132 0.000 0.000 0.840 0.028
#&gt; GSM28734     4  0.3106      0.952 0.140 0.000 0.000 0.840 0.020
#&gt; GSM28747     5  0.4283      0.294 0.348 0.000 0.000 0.008 0.644
#&gt; GSM11257     5  0.3825      0.660 0.060 0.000 0.000 0.136 0.804
#&gt; GSM11252     1  0.6034      0.579 0.572 0.000 0.000 0.172 0.256
#&gt; GSM11264     3  0.0000      0.885 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.3704      0.815 0.092 0.000 0.820 0.088 0.000
#&gt; GSM11258     4  0.3642      0.871 0.232 0.000 0.000 0.760 0.008
#&gt; GSM28728     5  0.0898      0.774 0.020 0.000 0.000 0.008 0.972
#&gt; GSM28746     5  0.6085     -0.187 0.404 0.000 0.000 0.124 0.472
#&gt; GSM28738     5  0.1549      0.752 0.040 0.000 0.000 0.016 0.944
#&gt; GSM28741     5  0.4271      0.597 0.024 0.180 0.000 0.024 0.772
#&gt; GSM28729     5  0.1281      0.774 0.032 0.000 0.000 0.012 0.956
#&gt; GSM28742     5  0.0807      0.773 0.012 0.000 0.000 0.012 0.976
#&gt; GSM11250     2  0.0579      0.986 0.008 0.984 0.000 0.008 0.000
#&gt; GSM11245     1  0.6023      0.581 0.576 0.000 0.000 0.176 0.248
#&gt; GSM11246     1  0.3177      0.851 0.792 0.000 0.000 0.000 0.208
#&gt; GSM11261     5  0.6107      0.431 0.144 0.000 0.012 0.244 0.600
#&gt; GSM11248     3  0.4159      0.746 0.068 0.000 0.776 0.156 0.000
#&gt; GSM28732     5  0.1282      0.769 0.044 0.000 0.000 0.004 0.952
#&gt; GSM11255     1  0.4087      0.769 0.756 0.000 0.000 0.036 0.208
#&gt; GSM28731     5  0.4387      0.293 0.348 0.000 0.000 0.012 0.640
#&gt; GSM28727     5  0.3671      0.555 0.236 0.000 0.000 0.008 0.756
#&gt; GSM11251     5  0.4003      0.455 0.288 0.000 0.000 0.008 0.704
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.2213     0.6170 0.008 0.000 0.000 0.004 0.888 0.100
#&gt; GSM28736     5  0.2213     0.6170 0.008 0.000 0.000 0.004 0.888 0.100
#&gt; GSM28737     1  0.1610     0.7443 0.916 0.000 0.000 0.000 0.084 0.000
#&gt; GSM11249     3  0.5560     0.5159 0.056 0.000 0.588 0.056 0.000 0.300
#&gt; GSM28745     2  0.0000     0.9965 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9965 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0146     0.9939 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9965 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9965 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9965 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9965 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9965 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9965 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.1531     0.7577 0.928 0.000 0.000 0.004 0.068 0.000
#&gt; GSM28739     1  0.1531     0.7577 0.928 0.000 0.000 0.004 0.068 0.000
#&gt; GSM11243     3  0.3582     0.6944 0.000 0.000 0.732 0.016 0.000 0.252
#&gt; GSM28740     1  0.1531     0.7577 0.928 0.000 0.000 0.004 0.068 0.000
#&gt; GSM11259     5  0.2003     0.6314 0.044 0.000 0.000 0.000 0.912 0.044
#&gt; GSM28726     5  0.2791     0.6108 0.008 0.000 0.000 0.016 0.852 0.124
#&gt; GSM28743     1  0.1531     0.7577 0.928 0.000 0.000 0.004 0.068 0.000
#&gt; GSM11256     4  0.1219     0.9697 0.048 0.000 0.000 0.948 0.004 0.000
#&gt; GSM11262     1  0.1531     0.7577 0.928 0.000 0.000 0.004 0.068 0.000
#&gt; GSM28724     5  0.4171     0.4022 0.040 0.000 0.000 0.008 0.716 0.236
#&gt; GSM28725     3  0.0000     0.8022 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000     0.8022 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000     0.8022 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.1219     0.9697 0.048 0.000 0.000 0.948 0.004 0.000
#&gt; GSM28734     4  0.1219     0.9697 0.048 0.000 0.000 0.948 0.004 0.000
#&gt; GSM28747     5  0.4637     0.2022 0.308 0.000 0.000 0.000 0.628 0.064
#&gt; GSM11257     5  0.5166     0.0718 0.012 0.000 0.000 0.060 0.528 0.400
#&gt; GSM11252     1  0.6233     0.0838 0.460 0.000 0.000 0.044 0.120 0.376
#&gt; GSM11264     3  0.0000     0.8022 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.3582     0.6944 0.000 0.000 0.732 0.016 0.000 0.252
#&gt; GSM11258     4  0.2048     0.9082 0.120 0.000 0.000 0.880 0.000 0.000
#&gt; GSM28728     5  0.2070     0.6100 0.008 0.000 0.000 0.000 0.892 0.100
#&gt; GSM28746     6  0.6666     0.1726 0.304 0.000 0.000 0.028 0.324 0.344
#&gt; GSM28738     5  0.4138     0.4206 0.012 0.000 0.000 0.012 0.664 0.312
#&gt; GSM28741     5  0.4802     0.4119 0.008 0.132 0.000 0.016 0.724 0.120
#&gt; GSM28729     5  0.3030     0.5905 0.008 0.000 0.000 0.008 0.816 0.168
#&gt; GSM28742     5  0.2658     0.6176 0.008 0.000 0.000 0.016 0.864 0.112
#&gt; GSM11250     2  0.0964     0.9718 0.004 0.968 0.000 0.012 0.000 0.016
#&gt; GSM11245     1  0.6299     0.0885 0.460 0.000 0.000 0.052 0.116 0.372
#&gt; GSM11246     1  0.1556     0.7492 0.920 0.000 0.000 0.000 0.080 0.000
#&gt; GSM11261     6  0.5034     0.1775 0.004 0.000 0.000 0.080 0.328 0.588
#&gt; GSM11248     3  0.5575     0.5112 0.056 0.000 0.584 0.056 0.000 0.304
#&gt; GSM28732     5  0.2190     0.6251 0.040 0.000 0.000 0.000 0.900 0.060
#&gt; GSM11255     1  0.5119     0.2444 0.552 0.000 0.000 0.008 0.068 0.372
#&gt; GSM28731     5  0.5169     0.1772 0.292 0.000 0.000 0.000 0.588 0.120
#&gt; GSM28727     5  0.2573     0.5750 0.132 0.000 0.000 0.004 0.856 0.008
#&gt; GSM11251     5  0.3329     0.4365 0.236 0.000 0.000 0.004 0.756 0.004
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:kmeans 44     0.387 2
#> SD:kmeans 49     0.368 3
#> SD:kmeans 41     0.407 4
#> SD:kmeans 45     0.483 5
#> SD:kmeans 38     0.472 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4708 0.530   0.530
#> 3 3 0.825           0.913       0.961         0.3542 0.746   0.557
#> 4 4 0.802           0.843       0.892         0.1763 0.789   0.484
#> 5 5 0.846           0.815       0.904         0.0711 0.949   0.797
#> 6 6 0.841           0.753       0.808         0.0393 0.980   0.903
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.0000      1.000 1.000 0.000
#&gt; GSM28736     1  0.0376      0.996 0.996 0.004
#&gt; GSM28737     1  0.0000      1.000 1.000 0.000
#&gt; GSM11249     1  0.0000      1.000 1.000 0.000
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000
#&gt; GSM11265     1  0.0000      1.000 1.000 0.000
#&gt; GSM28739     1  0.0000      1.000 1.000 0.000
#&gt; GSM11243     2  0.0000      1.000 0.000 1.000
#&gt; GSM28740     1  0.0000      1.000 1.000 0.000
#&gt; GSM11259     1  0.0000      1.000 1.000 0.000
#&gt; GSM28726     1  0.0000      1.000 1.000 0.000
#&gt; GSM28743     1  0.0000      1.000 1.000 0.000
#&gt; GSM11256     1  0.0000      1.000 1.000 0.000
#&gt; GSM11262     1  0.0000      1.000 1.000 0.000
#&gt; GSM28724     1  0.0000      1.000 1.000 0.000
#&gt; GSM28725     2  0.0000      1.000 0.000 1.000
#&gt; GSM11263     2  0.0000      1.000 0.000 1.000
#&gt; GSM11267     2  0.0000      1.000 0.000 1.000
#&gt; GSM28744     1  0.0000      1.000 1.000 0.000
#&gt; GSM28734     1  0.0000      1.000 1.000 0.000
#&gt; GSM28747     1  0.0000      1.000 1.000 0.000
#&gt; GSM11257     1  0.0000      1.000 1.000 0.000
#&gt; GSM11252     1  0.0000      1.000 1.000 0.000
#&gt; GSM11264     2  0.0000      1.000 0.000 1.000
#&gt; GSM11247     2  0.0000      1.000 0.000 1.000
#&gt; GSM11258     1  0.0000      1.000 1.000 0.000
#&gt; GSM28728     1  0.0000      1.000 1.000 0.000
#&gt; GSM28746     1  0.0000      1.000 1.000 0.000
#&gt; GSM28738     1  0.0000      1.000 1.000 0.000
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000
#&gt; GSM28729     1  0.0000      1.000 1.000 0.000
#&gt; GSM28742     1  0.0000      1.000 1.000 0.000
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000
#&gt; GSM11245     1  0.0000      1.000 1.000 0.000
#&gt; GSM11246     1  0.0000      1.000 1.000 0.000
#&gt; GSM11261     2  0.0000      1.000 0.000 1.000
#&gt; GSM11248     1  0.0000      1.000 1.000 0.000
#&gt; GSM28732     1  0.0000      1.000 1.000 0.000
#&gt; GSM11255     1  0.0000      1.000 1.000 0.000
#&gt; GSM28731     1  0.0000      1.000 1.000 0.000
#&gt; GSM28727     1  0.0000      1.000 1.000 0.000
#&gt; GSM11251     1  0.0000      1.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM28736     2  0.0747      0.980 0.016 0.984 0.000
#&gt; GSM28737     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM11249     3  0.0000      0.915 0.000 0.000 1.000
#&gt; GSM28745     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11244     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28748     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11266     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28730     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11253     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11254     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11260     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28733     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11265     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM11243     3  0.0000      0.915 0.000 0.000 1.000
#&gt; GSM28740     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM11259     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM28726     1  0.4235      0.783 0.824 0.176 0.000
#&gt; GSM28743     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM11256     3  0.4931      0.727 0.232 0.000 0.768
#&gt; GSM11262     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM28724     1  0.4654      0.733 0.792 0.000 0.208
#&gt; GSM28725     3  0.0000      0.915 0.000 0.000 1.000
#&gt; GSM11263     3  0.0000      0.915 0.000 0.000 1.000
#&gt; GSM11267     3  0.0000      0.915 0.000 0.000 1.000
#&gt; GSM28744     3  0.4931      0.727 0.232 0.000 0.768
#&gt; GSM28734     3  0.1753      0.888 0.048 0.000 0.952
#&gt; GSM28747     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM11257     1  0.3116      0.856 0.892 0.000 0.108
#&gt; GSM11252     1  0.5785      0.454 0.668 0.000 0.332
#&gt; GSM11264     3  0.0000      0.915 0.000 0.000 1.000
#&gt; GSM11247     3  0.0000      0.915 0.000 0.000 1.000
#&gt; GSM11258     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM28728     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM28746     1  0.1964      0.906 0.944 0.000 0.056
#&gt; GSM28738     1  0.0747      0.939 0.984 0.016 0.000
#&gt; GSM28741     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28729     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM28742     1  0.4121      0.793 0.832 0.168 0.000
#&gt; GSM11250     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11245     3  0.5835      0.535 0.340 0.000 0.660
#&gt; GSM11246     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM11261     3  0.0000      0.915 0.000 0.000 1.000
#&gt; GSM11248     3  0.0000      0.915 0.000 0.000 1.000
#&gt; GSM28732     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM11255     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM28731     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.951 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.951 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.0469      0.770 0.988 0.000 0.000 0.012
#&gt; GSM28736     1  0.1004      0.753 0.972 0.024 0.000 0.004
#&gt; GSM28737     4  0.1389      0.846 0.048 0.000 0.000 0.952
#&gt; GSM11249     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11265     4  0.0921      0.858 0.028 0.000 0.000 0.972
#&gt; GSM28739     4  0.0921      0.858 0.028 0.000 0.000 0.972
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28740     4  0.0921      0.858 0.028 0.000 0.000 0.972
#&gt; GSM11259     1  0.4072      0.742 0.748 0.000 0.000 0.252
#&gt; GSM28726     1  0.0779      0.776 0.980 0.004 0.000 0.016
#&gt; GSM28743     4  0.0921      0.858 0.028 0.000 0.000 0.972
#&gt; GSM11256     4  0.5206      0.577 0.308 0.000 0.024 0.668
#&gt; GSM11262     4  0.0921      0.858 0.028 0.000 0.000 0.972
#&gt; GSM28724     1  0.6614      0.553 0.548 0.000 0.092 0.360
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.5161      0.587 0.300 0.000 0.024 0.676
#&gt; GSM28734     4  0.5964      0.613 0.208 0.000 0.108 0.684
#&gt; GSM28747     1  0.4776      0.594 0.624 0.000 0.000 0.376
#&gt; GSM11257     1  0.5149      0.291 0.648 0.000 0.016 0.336
#&gt; GSM11252     4  0.3933      0.653 0.200 0.000 0.008 0.792
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11258     4  0.1211      0.832 0.040 0.000 0.000 0.960
#&gt; GSM28728     1  0.1716      0.781 0.936 0.000 0.000 0.064
#&gt; GSM28746     4  0.2805      0.791 0.100 0.000 0.012 0.888
#&gt; GSM28738     1  0.0336      0.763 0.992 0.000 0.000 0.008
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28729     1  0.1118      0.780 0.964 0.000 0.000 0.036
#&gt; GSM28742     1  0.0707      0.777 0.980 0.000 0.000 0.020
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11245     4  0.4364      0.749 0.092 0.000 0.092 0.816
#&gt; GSM11246     4  0.0921      0.858 0.028 0.000 0.000 0.972
#&gt; GSM11261     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11248     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28732     1  0.4103      0.740 0.744 0.000 0.000 0.256
#&gt; GSM11255     4  0.0592      0.855 0.016 0.000 0.000 0.984
#&gt; GSM28731     1  0.4356      0.712 0.708 0.000 0.000 0.292
#&gt; GSM28727     1  0.4222      0.729 0.728 0.000 0.000 0.272
#&gt; GSM11251     1  0.4222      0.729 0.728 0.000 0.000 0.272
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.1952      0.769 0.004 0.000 0.000 0.084 0.912
#&gt; GSM28736     5  0.1892      0.768 0.000 0.004 0.000 0.080 0.916
#&gt; GSM28737     1  0.0162      0.824 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11249     3  0.0794      0.973 0.000 0.000 0.972 0.028 0.000
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000      0.826 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.826 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28740     1  0.0000      0.826 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     5  0.2329      0.786 0.124 0.000 0.000 0.000 0.876
#&gt; GSM28726     5  0.1197      0.780 0.000 0.000 0.000 0.048 0.952
#&gt; GSM28743     1  0.0000      0.826 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.0290      0.879 0.008 0.000 0.000 0.992 0.000
#&gt; GSM11262     1  0.0000      0.826 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28724     5  0.7145      0.399 0.212 0.000 0.040 0.248 0.500
#&gt; GSM28725     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     4  0.0290      0.879 0.008 0.000 0.000 0.992 0.000
#&gt; GSM28734     4  0.0794      0.870 0.028 0.000 0.000 0.972 0.000
#&gt; GSM28747     5  0.4781      0.387 0.428 0.000 0.000 0.020 0.552
#&gt; GSM11257     4  0.2011      0.827 0.004 0.000 0.000 0.908 0.088
#&gt; GSM11252     1  0.6144      0.286 0.496 0.000 0.008 0.392 0.104
#&gt; GSM11264     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11258     4  0.3837      0.525 0.308 0.000 0.000 0.692 0.000
#&gt; GSM28728     5  0.1579      0.790 0.032 0.000 0.000 0.024 0.944
#&gt; GSM28746     1  0.6092      0.167 0.464 0.000 0.000 0.412 0.124
#&gt; GSM28738     5  0.3210      0.650 0.000 0.000 0.000 0.212 0.788
#&gt; GSM28741     2  0.0162      0.995 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28729     5  0.1082      0.786 0.008 0.000 0.000 0.028 0.964
#&gt; GSM28742     5  0.1121      0.780 0.000 0.000 0.000 0.044 0.956
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11245     1  0.6245      0.255 0.496 0.000 0.048 0.408 0.048
#&gt; GSM11246     1  0.0162      0.824 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11261     3  0.0162      0.991 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11248     3  0.0510      0.983 0.000 0.000 0.984 0.016 0.000
#&gt; GSM28732     5  0.2338      0.787 0.112 0.000 0.000 0.004 0.884
#&gt; GSM11255     1  0.2522      0.769 0.896 0.000 0.004 0.076 0.024
#&gt; GSM28731     5  0.4430      0.534 0.360 0.000 0.000 0.012 0.628
#&gt; GSM28727     5  0.3210      0.736 0.212 0.000 0.000 0.000 0.788
#&gt; GSM11251     5  0.3707      0.670 0.284 0.000 0.000 0.000 0.716
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.3978      0.551 0.000 0.000 0.000 0.032 0.700 0.268
#&gt; GSM28736     5  0.4060      0.547 0.000 0.000 0.000 0.032 0.684 0.284
#&gt; GSM28737     1  0.0000      0.910 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11249     3  0.3279      0.783 0.000 0.000 0.796 0.028 0.000 0.176
#&gt; GSM28745     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000      0.910 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.910 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0000      0.950 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28740     1  0.0000      0.910 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     5  0.3308      0.606 0.072 0.000 0.000 0.004 0.828 0.096
#&gt; GSM28726     5  0.3482      0.569 0.000 0.000 0.000 0.000 0.684 0.316
#&gt; GSM28743     1  0.0146      0.908 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM11256     4  0.0146      0.818 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM11262     1  0.0146      0.908 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM28724     5  0.7454      0.223 0.108 0.000 0.040 0.140 0.480 0.232
#&gt; GSM28725     3  0.0000      0.950 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.950 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.950 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0146      0.818 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM28734     4  0.0291      0.815 0.004 0.000 0.000 0.992 0.000 0.004
#&gt; GSM28747     5  0.5572      0.332 0.288 0.000 0.000 0.004 0.552 0.156
#&gt; GSM11257     4  0.4092      0.584 0.004 0.000 0.000 0.740 0.060 0.196
#&gt; GSM11252     6  0.6347      0.651 0.224 0.000 0.004 0.264 0.020 0.488
#&gt; GSM11264     3  0.0000      0.950 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0000      0.950 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11258     4  0.2941      0.561 0.220 0.000 0.000 0.780 0.000 0.000
#&gt; GSM28728     5  0.3919      0.598 0.016 0.000 0.008 0.004 0.728 0.244
#&gt; GSM28746     6  0.7522      0.290 0.224 0.000 0.000 0.232 0.176 0.368
#&gt; GSM28738     5  0.5259      0.468 0.000 0.000 0.000 0.096 0.468 0.436
#&gt; GSM28741     2  0.0520      0.983 0.000 0.984 0.000 0.000 0.008 0.008
#&gt; GSM28729     5  0.3944      0.519 0.000 0.000 0.000 0.004 0.568 0.428
#&gt; GSM28742     5  0.3578      0.565 0.000 0.000 0.000 0.000 0.660 0.340
#&gt; GSM11250     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11245     6  0.6425      0.646 0.208 0.000 0.008 0.280 0.020 0.484
#&gt; GSM11246     1  0.0146      0.906 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM11261     3  0.0146      0.947 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM11248     3  0.2932      0.809 0.000 0.000 0.820 0.016 0.000 0.164
#&gt; GSM28732     5  0.3727      0.552 0.040 0.000 0.000 0.004 0.768 0.188
#&gt; GSM11255     1  0.4868     -0.115 0.548 0.000 0.000 0.044 0.008 0.400
#&gt; GSM28731     5  0.5837      0.384 0.212 0.000 0.000 0.008 0.536 0.244
#&gt; GSM28727     5  0.3279      0.564 0.176 0.000 0.000 0.000 0.796 0.028
#&gt; GSM11251     5  0.3695      0.519 0.244 0.000 0.000 0.000 0.732 0.024
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> SD:skmeans 50     0.394 2
#> SD:skmeans 49     0.449 3
#> SD:skmeans 49     0.348 4
#> SD:skmeans 45     0.471 5
#> SD:skmeans 44     0.440 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3509 0.650   0.650
#> 3 3 1.000           0.999       1.000         0.5752 0.798   0.688
#> 4 4 1.000           0.979       0.990         0.1658 0.912   0.803
#> 5 5 0.780           0.743       0.890         0.1341 0.925   0.791
#> 6 6 0.797           0.800       0.863         0.0758 0.930   0.760
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2
#&gt; GSM28735     1       0          1  1  0
#&gt; GSM28736     1       0          1  1  0
#&gt; GSM28737     1       0          1  1  0
#&gt; GSM11249     1       0          1  1  0
#&gt; GSM28745     2       0          1  0  1
#&gt; GSM11244     2       0          1  0  1
#&gt; GSM28748     2       0          1  0  1
#&gt; GSM11266     2       0          1  0  1
#&gt; GSM28730     2       0          1  0  1
#&gt; GSM11253     2       0          1  0  1
#&gt; GSM11254     2       0          1  0  1
#&gt; GSM11260     2       0          1  0  1
#&gt; GSM28733     2       0          1  0  1
#&gt; GSM11265     1       0          1  1  0
#&gt; GSM28739     1       0          1  1  0
#&gt; GSM11243     1       0          1  1  0
#&gt; GSM28740     1       0          1  1  0
#&gt; GSM11259     1       0          1  1  0
#&gt; GSM28726     1       0          1  1  0
#&gt; GSM28743     1       0          1  1  0
#&gt; GSM11256     1       0          1  1  0
#&gt; GSM11262     1       0          1  1  0
#&gt; GSM28724     1       0          1  1  0
#&gt; GSM28725     1       0          1  1  0
#&gt; GSM11263     1       0          1  1  0
#&gt; GSM11267     1       0          1  1  0
#&gt; GSM28744     1       0          1  1  0
#&gt; GSM28734     1       0          1  1  0
#&gt; GSM28747     1       0          1  1  0
#&gt; GSM11257     1       0          1  1  0
#&gt; GSM11252     1       0          1  1  0
#&gt; GSM11264     1       0          1  1  0
#&gt; GSM11247     1       0          1  1  0
#&gt; GSM11258     1       0          1  1  0
#&gt; GSM28728     1       0          1  1  0
#&gt; GSM28746     1       0          1  1  0
#&gt; GSM28738     1       0          1  1  0
#&gt; GSM28741     2       0          1  0  1
#&gt; GSM28729     1       0          1  1  0
#&gt; GSM28742     1       0          1  1  0
#&gt; GSM11250     2       0          1  0  1
#&gt; GSM11245     1       0          1  1  0
#&gt; GSM11246     1       0          1  1  0
#&gt; GSM11261     1       0          1  1  0
#&gt; GSM11248     1       0          1  1  0
#&gt; GSM28732     1       0          1  1  0
#&gt; GSM11255     1       0          1  1  0
#&gt; GSM28731     1       0          1  1  0
#&gt; GSM28727     1       0          1  1  0
#&gt; GSM11251     1       0          1  1  0
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3
#&gt; GSM28735     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28736     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28737     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11249     3   0.000      0.995 0.000  0 1.000
#&gt; GSM28745     2   0.000      1.000 0.000  1 0.000
#&gt; GSM11244     2   0.000      1.000 0.000  1 0.000
#&gt; GSM28748     2   0.000      1.000 0.000  1 0.000
#&gt; GSM11266     2   0.000      1.000 0.000  1 0.000
#&gt; GSM28730     2   0.000      1.000 0.000  1 0.000
#&gt; GSM11253     2   0.000      1.000 0.000  1 0.000
#&gt; GSM11254     2   0.000      1.000 0.000  1 0.000
#&gt; GSM11260     2   0.000      1.000 0.000  1 0.000
#&gt; GSM28733     2   0.000      1.000 0.000  1 0.000
#&gt; GSM11265     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28739     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11243     3   0.000      0.995 0.000  0 1.000
#&gt; GSM28740     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11259     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28726     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28743     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11256     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11262     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28724     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28725     3   0.000      0.995 0.000  0 1.000
#&gt; GSM11263     3   0.000      0.995 0.000  0 1.000
#&gt; GSM11267     3   0.000      0.995 0.000  0 1.000
#&gt; GSM28744     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28734     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28747     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11257     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11252     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11264     3   0.000      0.995 0.000  0 1.000
#&gt; GSM11247     3   0.000      0.995 0.000  0 1.000
#&gt; GSM11258     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28728     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28746     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28738     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28741     2   0.000      1.000 0.000  1 0.000
#&gt; GSM28729     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28742     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11250     2   0.000      1.000 0.000  1 0.000
#&gt; GSM11245     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11246     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11261     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11248     3   0.103      0.965 0.024  0 0.976
#&gt; GSM28732     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11255     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28731     1   0.000      1.000 1.000  0 0.000
#&gt; GSM28727     1   0.000      1.000 1.000  0 0.000
#&gt; GSM11251     1   0.000      1.000 1.000  0 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4
#&gt; GSM28735     1  0.0592      0.982 0.984  0 0.000 0.016
#&gt; GSM28736     1  0.0707      0.980 0.980  0 0.000 0.020
#&gt; GSM28737     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM11249     3  0.0707      0.972 0.000  0 0.980 0.020
#&gt; GSM28745     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11265     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM28739     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM11243     3  0.0000      0.987 0.000  0 1.000 0.000
#&gt; GSM28740     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM11259     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM28726     1  0.0336      0.985 0.992  0 0.000 0.008
#&gt; GSM28743     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM11256     4  0.0000      0.955 0.000  0 0.000 1.000
#&gt; GSM11262     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM28724     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM28725     3  0.0000      0.987 0.000  0 1.000 0.000
#&gt; GSM11263     3  0.0000      0.987 0.000  0 1.000 0.000
#&gt; GSM11267     3  0.0000      0.987 0.000  0 1.000 0.000
#&gt; GSM28744     4  0.0188      0.958 0.004  0 0.000 0.996
#&gt; GSM28734     4  0.0188      0.958 0.004  0 0.000 0.996
#&gt; GSM28747     1  0.0336      0.985 0.992  0 0.000 0.008
#&gt; GSM11257     1  0.3486      0.781 0.812  0 0.000 0.188
#&gt; GSM11252     1  0.0707      0.980 0.980  0 0.000 0.020
#&gt; GSM11264     3  0.0000      0.987 0.000  0 1.000 0.000
#&gt; GSM11247     3  0.0000      0.987 0.000  0 1.000 0.000
#&gt; GSM11258     4  0.2081      0.880 0.084  0 0.000 0.916
#&gt; GSM28728     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM28746     1  0.0592      0.982 0.984  0 0.000 0.016
#&gt; GSM28738     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM28741     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM28729     1  0.0336      0.985 0.992  0 0.000 0.008
#&gt; GSM28742     1  0.0707      0.980 0.980  0 0.000 0.020
#&gt; GSM11250     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11245     1  0.0707      0.980 0.980  0 0.000 0.020
#&gt; GSM11246     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM11261     1  0.0336      0.985 0.992  0 0.000 0.008
#&gt; GSM11248     3  0.1624      0.935 0.028  0 0.952 0.020
#&gt; GSM28732     1  0.0707      0.980 0.980  0 0.000 0.020
#&gt; GSM11255     1  0.0188      0.986 0.996  0 0.000 0.004
#&gt; GSM28731     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM28727     1  0.0000      0.987 1.000  0 0.000 0.000
#&gt; GSM11251     1  0.0000      0.987 1.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3    p4    p5
#&gt; GSM28735     1   0.366     0.5898 0.724 0.00 0.000 0.000 0.276
#&gt; GSM28736     5   0.228     0.5566 0.120 0.00 0.000 0.000 0.880
#&gt; GSM28737     1   0.000     0.7982 1.000 0.00 0.000 0.000 0.000
#&gt; GSM11249     3   0.426     0.4887 0.000 0.00 0.564 0.000 0.436
#&gt; GSM28745     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11244     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28748     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11266     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28730     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11253     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11254     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11260     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28733     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11265     1   0.000     0.7982 1.000 0.00 0.000 0.000 0.000
#&gt; GSM28739     1   0.000     0.7982 1.000 0.00 0.000 0.000 0.000
#&gt; GSM11243     3   0.000     0.8616 0.000 0.00 1.000 0.000 0.000
#&gt; GSM28740     1   0.000     0.7982 1.000 0.00 0.000 0.000 0.000
#&gt; GSM11259     1   0.233     0.7706 0.876 0.00 0.000 0.000 0.124
#&gt; GSM28726     5   0.410     0.4669 0.372 0.00 0.000 0.000 0.628
#&gt; GSM28743     1   0.000     0.7982 1.000 0.00 0.000 0.000 0.000
#&gt; GSM11256     4   0.000     0.9508 0.000 0.00 0.000 1.000 0.000
#&gt; GSM11262     1   0.000     0.7982 1.000 0.00 0.000 0.000 0.000
#&gt; GSM28724     1   0.191     0.7897 0.908 0.00 0.000 0.000 0.092
#&gt; GSM28725     3   0.000     0.8616 0.000 0.00 1.000 0.000 0.000
#&gt; GSM11263     3   0.000     0.8616 0.000 0.00 1.000 0.000 0.000
#&gt; GSM11267     3   0.000     0.8616 0.000 0.00 1.000 0.000 0.000
#&gt; GSM28744     4   0.000     0.9508 0.000 0.00 0.000 1.000 0.000
#&gt; GSM28734     4   0.000     0.9508 0.000 0.00 0.000 1.000 0.000
#&gt; GSM28747     1   0.252     0.7649 0.860 0.00 0.000 0.000 0.140
#&gt; GSM11257     5   0.499     0.0490 0.416 0.00 0.000 0.032 0.552
#&gt; GSM11252     1   0.430    -0.0821 0.524 0.00 0.000 0.000 0.476
#&gt; GSM11264     3   0.000     0.8616 0.000 0.00 1.000 0.000 0.000
#&gt; GSM11247     3   0.000     0.8616 0.000 0.00 1.000 0.000 0.000
#&gt; GSM11258     4   0.223     0.8403 0.116 0.00 0.000 0.884 0.000
#&gt; GSM28728     1   0.104     0.8038 0.960 0.00 0.000 0.000 0.040
#&gt; GSM28746     1   0.297     0.5945 0.816 0.00 0.000 0.000 0.184
#&gt; GSM28738     1   0.273     0.7447 0.840 0.00 0.000 0.000 0.160
#&gt; GSM28741     2   0.413     0.5246 0.000 0.62 0.000 0.000 0.380
#&gt; GSM28729     1   0.218     0.7859 0.888 0.00 0.000 0.000 0.112
#&gt; GSM28742     5   0.337     0.6403 0.232 0.00 0.000 0.000 0.768
#&gt; GSM11250     2   0.000     0.9621 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11245     1   0.429    -0.0740 0.540 0.00 0.000 0.000 0.460
#&gt; GSM11246     1   0.104     0.8038 0.960 0.00 0.000 0.000 0.040
#&gt; GSM11261     1   0.088     0.7901 0.968 0.00 0.000 0.000 0.032
#&gt; GSM11248     3   0.440     0.4815 0.004 0.00 0.560 0.000 0.436
#&gt; GSM28732     1   0.377     0.5271 0.704 0.00 0.000 0.000 0.296
#&gt; GSM11255     1   0.324     0.4918 0.784 0.00 0.000 0.000 0.216
#&gt; GSM28731     1   0.104     0.8038 0.960 0.00 0.000 0.000 0.040
#&gt; GSM28727     1   0.238     0.7701 0.872 0.00 0.000 0.000 0.128
#&gt; GSM11251     1   0.233     0.7706 0.876 0.00 0.000 0.000 0.124
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     1  0.2491      0.534 0.836 0.000 0.000 0.000 0.164 0.000
#&gt; GSM28736     5  0.3927      0.899 0.344 0.000 0.000 0.000 0.644 0.012
#&gt; GSM28737     1  0.3446      0.716 0.692 0.000 0.000 0.000 0.308 0.000
#&gt; GSM11249     6  0.0790      0.856 0.000 0.000 0.032 0.000 0.000 0.968
#&gt; GSM28745     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3446      0.716 0.692 0.000 0.000 0.000 0.308 0.000
#&gt; GSM28739     1  0.3446      0.716 0.692 0.000 0.000 0.000 0.308 0.000
#&gt; GSM11243     3  0.0458      0.988 0.000 0.000 0.984 0.000 0.000 0.016
#&gt; GSM28740     1  0.3446      0.716 0.692 0.000 0.000 0.000 0.308 0.000
#&gt; GSM11259     1  0.0146      0.737 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM28726     5  0.3634      0.899 0.356 0.000 0.000 0.000 0.644 0.000
#&gt; GSM28743     1  0.3446      0.716 0.692 0.000 0.000 0.000 0.308 0.000
#&gt; GSM11256     4  0.0000      0.865 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11262     1  0.3446      0.716 0.692 0.000 0.000 0.000 0.308 0.000
#&gt; GSM28724     1  0.1967      0.753 0.904 0.000 0.000 0.000 0.084 0.012
#&gt; GSM28725     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0000      0.865 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28734     4  0.0000      0.865 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28747     1  0.0146      0.737 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM11257     6  0.3511      0.691 0.148 0.000 0.000 0.004 0.048 0.800
#&gt; GSM11252     6  0.0790      0.867 0.032 0.000 0.000 0.000 0.000 0.968
#&gt; GSM11264     3  0.0000      0.994 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0458      0.988 0.000 0.000 0.984 0.000 0.000 0.016
#&gt; GSM11258     4  0.4265      0.570 0.040 0.000 0.000 0.660 0.300 0.000
#&gt; GSM28728     1  0.0000      0.739 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28746     1  0.5449      0.602 0.572 0.000 0.000 0.000 0.240 0.188
#&gt; GSM28738     1  0.2968      0.521 0.816 0.000 0.000 0.000 0.168 0.016
#&gt; GSM28741     2  0.4702      0.104 0.044 0.496 0.000 0.000 0.460 0.000
#&gt; GSM28729     1  0.0692      0.738 0.976 0.000 0.000 0.000 0.004 0.020
#&gt; GSM28742     5  0.5624      0.805 0.356 0.000 0.000 0.000 0.488 0.156
#&gt; GSM11250     2  0.0000      0.949 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11245     6  0.0790      0.867 0.032 0.000 0.000 0.000 0.000 0.968
#&gt; GSM11246     1  0.2135      0.752 0.872 0.000 0.000 0.000 0.128 0.000
#&gt; GSM11261     1  0.4653      0.664 0.684 0.000 0.000 0.000 0.120 0.196
#&gt; GSM11248     6  0.0790      0.856 0.000 0.000 0.032 0.000 0.000 0.968
#&gt; GSM28732     1  0.2854      0.602 0.792 0.000 0.000 0.000 0.000 0.208
#&gt; GSM11255     6  0.2941      0.643 0.220 0.000 0.000 0.000 0.000 0.780
#&gt; GSM28731     1  0.0363      0.741 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM28727     1  0.0146      0.737 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM11251     1  0.0146      0.737 0.996 0.000 0.000 0.000 0.004 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> SD:pam 50     0.394 2
#> SD:pam 50     0.370 3
#> SD:pam 50     0.512 4
#> SD:pam 43     0.451 5
#> SD:pam 49     0.439 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.458           0.482       0.775         0.4558 0.503   0.503
#> 3 3 0.622           0.826       0.913         0.3358 0.603   0.376
#> 4 4 0.784           0.887       0.920         0.0839 0.864   0.691
#> 5 5 0.664           0.702       0.838         0.1410 0.819   0.537
#> 6 6 0.695           0.567       0.740         0.0526 0.855   0.470
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     2   0.983     -0.523 0.424 0.576
#&gt; GSM28736     2   0.595      0.220 0.144 0.856
#&gt; GSM28737     1   0.988      0.761 0.564 0.436
#&gt; GSM11249     1   0.000      0.376 1.000 0.000
#&gt; GSM28745     2   0.975      0.571 0.408 0.592
#&gt; GSM11244     2   0.975      0.571 0.408 0.592
#&gt; GSM28748     2   0.975      0.571 0.408 0.592
#&gt; GSM11266     2   0.975      0.571 0.408 0.592
#&gt; GSM28730     2   0.975      0.571 0.408 0.592
#&gt; GSM11253     2   0.975      0.571 0.408 0.592
#&gt; GSM11254     2   0.975      0.571 0.408 0.592
#&gt; GSM11260     2   0.975      0.571 0.408 0.592
#&gt; GSM28733     2   0.975      0.571 0.408 0.592
#&gt; GSM11265     1   0.988      0.761 0.564 0.436
#&gt; GSM28739     1   0.988      0.761 0.564 0.436
#&gt; GSM11243     1   0.000      0.376 1.000 0.000
#&gt; GSM28740     1   0.988      0.761 0.564 0.436
#&gt; GSM11259     1   0.988      0.761 0.564 0.436
#&gt; GSM28726     2   0.985     -0.531 0.428 0.572
#&gt; GSM28743     1   0.988      0.761 0.564 0.436
#&gt; GSM11256     2   0.714      0.273 0.196 0.804
#&gt; GSM11262     1   0.988      0.761 0.564 0.436
#&gt; GSM28724     1   0.988      0.761 0.564 0.436
#&gt; GSM28725     1   0.000      0.376 1.000 0.000
#&gt; GSM11263     1   0.000      0.376 1.000 0.000
#&gt; GSM11267     1   0.000      0.376 1.000 0.000
#&gt; GSM28744     2   0.714      0.273 0.196 0.804
#&gt; GSM28734     2   0.714      0.273 0.196 0.804
#&gt; GSM28747     1   0.988      0.761 0.564 0.436
#&gt; GSM11257     2   0.722      0.196 0.200 0.800
#&gt; GSM11252     1   0.988      0.761 0.564 0.436
#&gt; GSM11264     1   0.000      0.376 1.000 0.000
#&gt; GSM11247     1   0.000      0.376 1.000 0.000
#&gt; GSM11258     2   0.706      0.268 0.192 0.808
#&gt; GSM28728     1   0.988      0.761 0.564 0.436
#&gt; GSM28746     1   0.988      0.761 0.564 0.436
#&gt; GSM28738     2   0.988     -0.547 0.436 0.564
#&gt; GSM28741     2   0.416      0.418 0.084 0.916
#&gt; GSM28729     1   0.988      0.761 0.564 0.436
#&gt; GSM28742     2   0.993     -0.581 0.452 0.548
#&gt; GSM11250     2   0.975      0.571 0.408 0.592
#&gt; GSM11245     1   0.988      0.761 0.564 0.436
#&gt; GSM11246     1   0.988      0.761 0.564 0.436
#&gt; GSM11261     1   0.402      0.427 0.920 0.080
#&gt; GSM11248     1   0.000      0.376 1.000 0.000
#&gt; GSM28732     1   0.988      0.761 0.564 0.436
#&gt; GSM11255     1   0.988      0.761 0.564 0.436
#&gt; GSM28731     1   0.988      0.761 0.564 0.436
#&gt; GSM28727     1   0.988      0.761 0.564 0.436
#&gt; GSM11251     1   0.988      0.761 0.564 0.436
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1   0.547     0.8132 0.800 0.040 0.160
#&gt; GSM28736     1   0.614     0.7414 0.748 0.040 0.212
#&gt; GSM28737     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM11249     3   0.196     0.8407 0.056 0.000 0.944
#&gt; GSM28745     2   0.000     0.9394 0.000 1.000 0.000
#&gt; GSM11244     2   0.000     0.9394 0.000 1.000 0.000
#&gt; GSM28748     2   0.656     0.1652 0.008 0.576 0.416
#&gt; GSM11266     2   0.000     0.9394 0.000 1.000 0.000
#&gt; GSM28730     2   0.000     0.9394 0.000 1.000 0.000
#&gt; GSM11253     2   0.000     0.9394 0.000 1.000 0.000
#&gt; GSM11254     2   0.000     0.9394 0.000 1.000 0.000
#&gt; GSM11260     2   0.000     0.9394 0.000 1.000 0.000
#&gt; GSM28733     2   0.000     0.9394 0.000 1.000 0.000
#&gt; GSM11265     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM28739     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM11243     3   0.000     0.8265 0.000 0.000 1.000
#&gt; GSM28740     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM11259     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM28726     1   0.575     0.7894 0.780 0.040 0.180
#&gt; GSM28743     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM11256     3   0.355     0.8262 0.132 0.000 0.868
#&gt; GSM11262     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM28724     1   0.400     0.8385 0.840 0.000 0.160
#&gt; GSM28725     3   0.000     0.8265 0.000 0.000 1.000
#&gt; GSM11263     3   0.000     0.8265 0.000 0.000 1.000
#&gt; GSM11267     3   0.000     0.8265 0.000 0.000 1.000
#&gt; GSM28744     3   0.355     0.8262 0.132 0.000 0.868
#&gt; GSM28734     3   0.355     0.8262 0.132 0.000 0.868
#&gt; GSM28747     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM11257     3   0.525     0.6661 0.264 0.000 0.736
#&gt; GSM11252     1   0.388     0.8452 0.848 0.000 0.152
#&gt; GSM11264     3   0.000     0.8265 0.000 0.000 1.000
#&gt; GSM11247     3   0.000     0.8265 0.000 0.000 1.000
#&gt; GSM11258     3   0.355     0.8262 0.132 0.000 0.868
#&gt; GSM28728     1   0.400     0.8385 0.840 0.000 0.160
#&gt; GSM28746     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM28738     1   0.400     0.8385 0.840 0.000 0.160
#&gt; GSM28741     3   0.927     0.0857 0.416 0.156 0.428
#&gt; GSM28729     1   0.348     0.8591 0.872 0.000 0.128
#&gt; GSM28742     1   0.400     0.8385 0.840 0.000 0.160
#&gt; GSM11250     3   0.974     0.1895 0.236 0.336 0.428
#&gt; GSM11245     1   0.435     0.8120 0.816 0.000 0.184
#&gt; GSM11246     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM11261     3   0.277     0.8385 0.080 0.004 0.916
#&gt; GSM11248     3   0.196     0.8407 0.056 0.000 0.944
#&gt; GSM28732     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM11255     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM28731     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM28727     1   0.000     0.9123 1.000 0.000 0.000
#&gt; GSM11251     1   0.000     0.9123 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.3933      0.821 0.792 0.008 0.000 0.200
#&gt; GSM28736     1  0.4260      0.819 0.792 0.008 0.012 0.188
#&gt; GSM28737     1  0.0707      0.876 0.980 0.000 0.000 0.020
#&gt; GSM11249     3  0.2670      0.877 0.052 0.000 0.908 0.040
#&gt; GSM28745     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.1767      0.915 0.000 0.944 0.012 0.044
#&gt; GSM11266     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.1716      0.868 0.936 0.000 0.000 0.064
#&gt; GSM28739     1  0.2149      0.860 0.912 0.000 0.000 0.088
#&gt; GSM11243     3  0.0000      0.961 0.000 0.000 1.000 0.000
#&gt; GSM28740     1  0.2281      0.852 0.904 0.000 0.000 0.096
#&gt; GSM11259     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM28726     1  0.3933      0.821 0.792 0.008 0.000 0.200
#&gt; GSM28743     1  0.2216      0.853 0.908 0.000 0.000 0.092
#&gt; GSM11256     4  0.0817      0.990 0.000 0.000 0.024 0.976
#&gt; GSM11262     1  0.2408      0.849 0.896 0.000 0.000 0.104
#&gt; GSM28724     1  0.4059      0.839 0.788 0.000 0.012 0.200
#&gt; GSM28725     3  0.0000      0.961 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      0.961 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      0.961 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.0817      0.990 0.000 0.000 0.024 0.976
#&gt; GSM28734     4  0.0817      0.990 0.000 0.000 0.024 0.976
#&gt; GSM28747     1  0.0469      0.875 0.988 0.000 0.000 0.012
#&gt; GSM11257     1  0.4456      0.784 0.716 0.000 0.004 0.280
#&gt; GSM11252     1  0.2542      0.878 0.904 0.000 0.012 0.084
#&gt; GSM11264     3  0.0000      0.961 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0000      0.961 0.000 0.000 1.000 0.000
#&gt; GSM11258     4  0.0804      0.972 0.008 0.000 0.012 0.980
#&gt; GSM28728     1  0.3962      0.841 0.820 0.000 0.028 0.152
#&gt; GSM28746     1  0.1302      0.876 0.956 0.000 0.000 0.044
#&gt; GSM28738     1  0.3528      0.829 0.808 0.000 0.000 0.192
#&gt; GSM28741     1  0.7223      0.609 0.592 0.172 0.012 0.224
#&gt; GSM28729     1  0.3718      0.838 0.820 0.000 0.012 0.168
#&gt; GSM28742     1  0.3444      0.834 0.816 0.000 0.000 0.184
#&gt; GSM11250     2  0.3937      0.703 0.000 0.800 0.012 0.188
#&gt; GSM11245     1  0.2676      0.878 0.896 0.000 0.012 0.092
#&gt; GSM11246     1  0.1211      0.874 0.960 0.000 0.000 0.040
#&gt; GSM11261     1  0.5558      0.723 0.712 0.000 0.208 0.080
#&gt; GSM11248     3  0.2751      0.871 0.056 0.000 0.904 0.040
#&gt; GSM28732     1  0.2741      0.867 0.892 0.000 0.012 0.096
#&gt; GSM11255     1  0.1211      0.877 0.960 0.000 0.000 0.040
#&gt; GSM28731     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.874 1.000 0.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.874 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.3395     0.6950 0.236 0.000 0.000 0.000 0.764
#&gt; GSM28736     5  0.3210     0.6997 0.212 0.000 0.000 0.000 0.788
#&gt; GSM28737     1  0.1478     0.7878 0.936 0.000 0.000 0.000 0.064
#&gt; GSM11249     3  0.4941     0.5696 0.044 0.000 0.628 0.000 0.328
#&gt; GSM28745     2  0.0000     0.9334 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9334 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.4455     0.2838 0.000 0.588 0.008 0.000 0.404
#&gt; GSM11266     2  0.0000     0.9334 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9334 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9334 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9334 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9334 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9334 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.2782     0.7603 0.880 0.000 0.000 0.048 0.072
#&gt; GSM28739     1  0.3359     0.7486 0.840 0.000 0.000 0.052 0.108
#&gt; GSM11243     3  0.0000     0.8667 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28740     1  0.3375     0.7376 0.840 0.000 0.000 0.056 0.104
#&gt; GSM11259     1  0.1732     0.7691 0.920 0.000 0.000 0.000 0.080
#&gt; GSM28726     5  0.3242     0.6996 0.216 0.000 0.000 0.000 0.784
#&gt; GSM28743     1  0.3375     0.7376 0.840 0.000 0.000 0.056 0.104
#&gt; GSM11256     4  0.0162     0.9976 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11262     1  0.4088     0.6927 0.776 0.000 0.000 0.056 0.168
#&gt; GSM28724     1  0.4481    -0.0325 0.576 0.000 0.008 0.000 0.416
#&gt; GSM28725     3  0.0000     0.8667 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000     0.8667 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000     0.8667 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     4  0.0162     0.9976 0.000 0.000 0.000 0.996 0.004
#&gt; GSM28734     4  0.0290     0.9953 0.000 0.000 0.000 0.992 0.008
#&gt; GSM28747     1  0.1965     0.7627 0.904 0.000 0.000 0.000 0.096
#&gt; GSM11257     5  0.2561     0.6579 0.144 0.000 0.000 0.000 0.856
#&gt; GSM11252     5  0.4126     0.6137 0.380 0.000 0.000 0.000 0.620
#&gt; GSM11264     3  0.0000     0.8667 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.0000     0.8667 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11258     5  0.4074     0.3487 0.000 0.000 0.000 0.364 0.636
#&gt; GSM28728     5  0.4559     0.4016 0.480 0.000 0.008 0.000 0.512
#&gt; GSM28746     1  0.2813     0.6761 0.832 0.000 0.000 0.000 0.168
#&gt; GSM28738     5  0.4268     0.4943 0.444 0.000 0.000 0.000 0.556
#&gt; GSM28741     5  0.4412     0.6376 0.080 0.164 0.000 0.000 0.756
#&gt; GSM28729     1  0.4533    -0.2676 0.544 0.000 0.008 0.000 0.448
#&gt; GSM28742     5  0.4552     0.4351 0.468 0.000 0.008 0.000 0.524
#&gt; GSM11250     5  0.4201     0.3765 0.000 0.328 0.008 0.000 0.664
#&gt; GSM11245     5  0.4030     0.6360 0.352 0.000 0.000 0.000 0.648
#&gt; GSM11246     1  0.0510     0.7848 0.984 0.000 0.000 0.000 0.016
#&gt; GSM11261     5  0.4022     0.5624 0.104 0.000 0.100 0.000 0.796
#&gt; GSM11248     3  0.5131     0.5171 0.048 0.000 0.588 0.000 0.364
#&gt; GSM28732     1  0.2077     0.7538 0.908 0.000 0.008 0.000 0.084
#&gt; GSM11255     1  0.3395     0.5497 0.764 0.000 0.000 0.000 0.236
#&gt; GSM28731     1  0.0794     0.7824 0.972 0.000 0.000 0.000 0.028
#&gt; GSM28727     1  0.1270     0.7767 0.948 0.000 0.000 0.000 0.052
#&gt; GSM11251     1  0.1270     0.7767 0.948 0.000 0.000 0.000 0.052
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.5390    0.01841 0.128 0.000 0.000 0.000 0.532 0.340
#&gt; GSM28736     6  0.6076    0.14876 0.272 0.000 0.000 0.000 0.344 0.384
#&gt; GSM28737     1  0.3699    0.75862 0.660 0.000 0.000 0.000 0.336 0.004
#&gt; GSM11249     6  0.5071    0.00371 0.056 0.000 0.376 0.000 0.012 0.556
#&gt; GSM28745     2  0.0000    0.91824 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000    0.91824 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.2764    0.80315 0.000 0.864 0.028 0.000 0.008 0.100
#&gt; GSM11266     2  0.0000    0.91824 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000    0.91824 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000    0.91824 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000    0.91824 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000    0.91824 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000    0.91824 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3628    0.77633 0.720 0.000 0.000 0.008 0.268 0.004
#&gt; GSM28739     1  0.4416    0.64211 0.600 0.000 0.000 0.020 0.372 0.008
#&gt; GSM11243     3  0.1267    0.94072 0.000 0.000 0.940 0.000 0.000 0.060
#&gt; GSM28740     1  0.4592    0.77010 0.668 0.000 0.000 0.020 0.276 0.036
#&gt; GSM11259     1  0.3838    0.67319 0.552 0.000 0.000 0.000 0.448 0.000
#&gt; GSM28726     5  0.6197   -0.29706 0.268 0.000 0.000 0.004 0.376 0.352
#&gt; GSM28743     1  0.4509    0.77032 0.684 0.000 0.000 0.020 0.260 0.036
#&gt; GSM11256     4  0.0260    0.81110 0.008 0.000 0.000 0.992 0.000 0.000
#&gt; GSM11262     1  0.4158    0.71564 0.740 0.000 0.000 0.020 0.204 0.036
#&gt; GSM28724     5  0.3569    0.46274 0.164 0.000 0.008 0.000 0.792 0.036
#&gt; GSM28725     3  0.0000    0.97118 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000    0.97118 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000    0.97118 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0260    0.81110 0.008 0.000 0.000 0.992 0.000 0.000
#&gt; GSM28734     4  0.0146    0.80725 0.000 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28747     1  0.3309    0.73017 0.720 0.000 0.000 0.000 0.280 0.000
#&gt; GSM11257     6  0.5925   -0.07159 0.236 0.000 0.000 0.000 0.308 0.456
#&gt; GSM11252     5  0.4970    0.42859 0.224 0.000 0.004 0.000 0.652 0.120
#&gt; GSM11264     3  0.0000    0.97118 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.1267    0.94072 0.000 0.000 0.940 0.000 0.000 0.060
#&gt; GSM11258     4  0.5576    0.22108 0.012 0.000 0.000 0.512 0.104 0.372
#&gt; GSM28728     5  0.0858    0.51599 0.028 0.000 0.000 0.000 0.968 0.004
#&gt; GSM28746     5  0.4921   -0.16417 0.436 0.000 0.000 0.004 0.508 0.052
#&gt; GSM28738     5  0.1501    0.51337 0.000 0.000 0.000 0.000 0.924 0.076
#&gt; GSM28741     6  0.7292    0.24929 0.256 0.116 0.004 0.000 0.200 0.424
#&gt; GSM28729     5  0.1363    0.50975 0.028 0.000 0.004 0.004 0.952 0.012
#&gt; GSM28742     5  0.1204    0.51759 0.000 0.000 0.000 0.000 0.944 0.056
#&gt; GSM11250     2  0.6212    0.02246 0.064 0.440 0.004 0.000 0.072 0.420
#&gt; GSM11245     5  0.5350    0.41714 0.228 0.000 0.016 0.000 0.628 0.128
#&gt; GSM11246     1  0.3468    0.77381 0.712 0.000 0.000 0.000 0.284 0.004
#&gt; GSM11261     6  0.6154    0.27211 0.024 0.000 0.212 0.004 0.216 0.544
#&gt; GSM11248     6  0.5074    0.04469 0.060 0.000 0.356 0.000 0.012 0.572
#&gt; GSM28732     5  0.4411   -0.38997 0.356 0.000 0.028 0.004 0.612 0.000
#&gt; GSM11255     5  0.4905   -0.12081 0.420 0.000 0.000 0.004 0.524 0.052
#&gt; GSM28731     1  0.3823    0.72757 0.564 0.000 0.000 0.000 0.436 0.000
#&gt; GSM28727     1  0.3866    0.66257 0.516 0.000 0.000 0.000 0.484 0.000
#&gt; GSM11251     1  0.3857    0.68038 0.532 0.000 0.000 0.000 0.468 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:mclust 30     0.414 2
#> SD:mclust 47     0.440 3
#> SD:mclust 50     0.511 4
#> SD:mclust 42     0.494 5
#> SD:mclust 34     0.461 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.983       0.992         0.3858 0.607   0.607
#> 3 3 1.000           0.974       0.991         0.4994 0.731   0.588
#> 4 4 0.876           0.913       0.950         0.2138 0.882   0.726
#> 5 5 0.747           0.721       0.849         0.1135 0.900   0.686
#> 6 6 0.716           0.567       0.758         0.0521 0.885   0.554
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.0000      0.999 1.000 0.000
#&gt; GSM28736     2  0.5178      0.866 0.116 0.884
#&gt; GSM28737     1  0.0000      0.999 1.000 0.000
#&gt; GSM11249     1  0.0000      0.999 1.000 0.000
#&gt; GSM28745     2  0.0000      0.969 0.000 1.000
#&gt; GSM11244     2  0.0000      0.969 0.000 1.000
#&gt; GSM28748     2  0.0000      0.969 0.000 1.000
#&gt; GSM11266     2  0.0000      0.969 0.000 1.000
#&gt; GSM28730     2  0.0000      0.969 0.000 1.000
#&gt; GSM11253     2  0.0000      0.969 0.000 1.000
#&gt; GSM11254     2  0.0000      0.969 0.000 1.000
#&gt; GSM11260     2  0.0000      0.969 0.000 1.000
#&gt; GSM28733     2  0.0000      0.969 0.000 1.000
#&gt; GSM11265     1  0.0000      0.999 1.000 0.000
#&gt; GSM28739     1  0.0000      0.999 1.000 0.000
#&gt; GSM11243     1  0.0000      0.999 1.000 0.000
#&gt; GSM28740     1  0.0000      0.999 1.000 0.000
#&gt; GSM11259     1  0.0000      0.999 1.000 0.000
#&gt; GSM28726     2  0.8267      0.663 0.260 0.740
#&gt; GSM28743     1  0.0000      0.999 1.000 0.000
#&gt; GSM11256     1  0.0000      0.999 1.000 0.000
#&gt; GSM11262     1  0.0000      0.999 1.000 0.000
#&gt; GSM28724     1  0.0000      0.999 1.000 0.000
#&gt; GSM28725     1  0.0000      0.999 1.000 0.000
#&gt; GSM11263     1  0.0000      0.999 1.000 0.000
#&gt; GSM11267     1  0.0000      0.999 1.000 0.000
#&gt; GSM28744     1  0.0000      0.999 1.000 0.000
#&gt; GSM28734     1  0.0000      0.999 1.000 0.000
#&gt; GSM28747     1  0.0000      0.999 1.000 0.000
#&gt; GSM11257     1  0.0000      0.999 1.000 0.000
#&gt; GSM11252     1  0.0000      0.999 1.000 0.000
#&gt; GSM11264     1  0.0000      0.999 1.000 0.000
#&gt; GSM11247     1  0.0000      0.999 1.000 0.000
#&gt; GSM11258     1  0.0000      0.999 1.000 0.000
#&gt; GSM28728     1  0.0000      0.999 1.000 0.000
#&gt; GSM28746     1  0.0000      0.999 1.000 0.000
#&gt; GSM28738     1  0.0000      0.999 1.000 0.000
#&gt; GSM28741     2  0.0000      0.969 0.000 1.000
#&gt; GSM28729     1  0.0000      0.999 1.000 0.000
#&gt; GSM28742     1  0.1184      0.983 0.984 0.016
#&gt; GSM11250     2  0.0000      0.969 0.000 1.000
#&gt; GSM11245     1  0.0000      0.999 1.000 0.000
#&gt; GSM11246     1  0.0000      0.999 1.000 0.000
#&gt; GSM11261     1  0.0376      0.996 0.996 0.004
#&gt; GSM11248     1  0.0000      0.999 1.000 0.000
#&gt; GSM28732     1  0.0000      0.999 1.000 0.000
#&gt; GSM11255     1  0.0000      0.999 1.000 0.000
#&gt; GSM28731     1  0.0000      0.999 1.000 0.000
#&gt; GSM28727     1  0.0000      0.999 1.000 0.000
#&gt; GSM11251     1  0.0000      0.999 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28736     1  0.0747      0.969 0.984 0.016 0.000
#&gt; GSM28737     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11249     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28745     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11244     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM28748     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11266     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM28730     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11253     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11254     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11260     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM28733     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11265     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28740     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11259     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28726     1  0.0237      0.980 0.996 0.004 0.000
#&gt; GSM28743     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11256     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11262     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28724     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28744     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28734     1  0.6252      0.203 0.556 0.000 0.444
#&gt; GSM28747     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11257     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11252     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11258     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28728     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28746     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28738     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28741     2  0.0237      0.994 0.004 0.996 0.000
#&gt; GSM28729     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28742     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11250     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11245     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11246     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11261     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11248     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28732     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11255     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28731     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.984 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.4804      0.450 0.616 0.000 0.000 0.384
#&gt; GSM28736     4  0.7001      0.547 0.180 0.244 0.000 0.576
#&gt; GSM28737     1  0.0188      0.919 0.996 0.000 0.000 0.004
#&gt; GSM11249     3  0.0188      0.990 0.000 0.000 0.996 0.004
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.0336      0.918 0.992 0.000 0.000 0.008
#&gt; GSM28739     1  0.0469      0.916 0.988 0.000 0.000 0.012
#&gt; GSM11243     3  0.0707      0.982 0.000 0.000 0.980 0.020
#&gt; GSM28740     1  0.0469      0.919 0.988 0.000 0.000 0.012
#&gt; GSM11259     1  0.0469      0.917 0.988 0.000 0.000 0.012
#&gt; GSM28726     1  0.4677      0.768 0.776 0.048 0.000 0.176
#&gt; GSM28743     1  0.0592      0.919 0.984 0.000 0.000 0.016
#&gt; GSM11256     4  0.1004      0.902 0.024 0.000 0.004 0.972
#&gt; GSM11262     1  0.0817      0.915 0.976 0.000 0.000 0.024
#&gt; GSM28724     1  0.0817      0.913 0.976 0.000 0.000 0.024
#&gt; GSM28725     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.1004      0.902 0.024 0.000 0.004 0.972
#&gt; GSM28734     4  0.1004      0.885 0.004 0.000 0.024 0.972
#&gt; GSM28747     1  0.0817      0.915 0.976 0.000 0.000 0.024
#&gt; GSM11257     4  0.1389      0.899 0.048 0.000 0.000 0.952
#&gt; GSM11252     1  0.3123      0.829 0.844 0.000 0.000 0.156
#&gt; GSM11264     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.1151      0.972 0.008 0.000 0.968 0.024
#&gt; GSM11258     4  0.1557      0.895 0.056 0.000 0.000 0.944
#&gt; GSM28728     1  0.0817      0.913 0.976 0.000 0.000 0.024
#&gt; GSM28746     1  0.2469      0.860 0.892 0.000 0.000 0.108
#&gt; GSM28738     1  0.4008      0.719 0.756 0.000 0.000 0.244
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28729     1  0.2408      0.877 0.896 0.000 0.000 0.104
#&gt; GSM28742     1  0.3726      0.778 0.788 0.000 0.000 0.212
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11245     1  0.3668      0.794 0.808 0.000 0.004 0.188
#&gt; GSM11246     1  0.0188      0.918 0.996 0.000 0.000 0.004
#&gt; GSM11261     3  0.0336      0.989 0.000 0.000 0.992 0.008
#&gt; GSM11248     3  0.0188      0.990 0.000 0.000 0.996 0.004
#&gt; GSM28732     1  0.0592      0.918 0.984 0.000 0.000 0.016
#&gt; GSM11255     1  0.0592      0.918 0.984 0.000 0.000 0.016
#&gt; GSM28731     1  0.0336      0.918 0.992 0.000 0.000 0.008
#&gt; GSM28727     1  0.0188      0.919 0.996 0.000 0.000 0.004
#&gt; GSM11251     1  0.0188      0.919 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     1  0.5443      0.230 0.604 0.000 0.000 0.312 0.084
#&gt; GSM28736     4  0.7304      0.427 0.228 0.160 0.000 0.528 0.084
#&gt; GSM28737     1  0.3086      0.712 0.816 0.000 0.000 0.004 0.180
#&gt; GSM11249     3  0.1717      0.901 0.004 0.000 0.936 0.008 0.052
#&gt; GSM28745     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3109      0.698 0.800 0.000 0.000 0.000 0.200
#&gt; GSM28739     1  0.3366      0.680 0.768 0.000 0.000 0.000 0.232
#&gt; GSM11243     3  0.3774      0.616 0.000 0.000 0.704 0.000 0.296
#&gt; GSM28740     1  0.3242      0.691 0.784 0.000 0.000 0.000 0.216
#&gt; GSM11259     5  0.4283      0.354 0.456 0.000 0.000 0.000 0.544
#&gt; GSM28726     1  0.5284     -0.236 0.532 0.004 0.000 0.040 0.424
#&gt; GSM28743     1  0.3171      0.704 0.816 0.000 0.000 0.008 0.176
#&gt; GSM11256     4  0.0451      0.856 0.004 0.000 0.000 0.988 0.008
#&gt; GSM11262     1  0.3209      0.702 0.812 0.000 0.000 0.008 0.180
#&gt; GSM28724     5  0.4504      0.349 0.428 0.000 0.008 0.000 0.564
#&gt; GSM28725     3  0.0609      0.923 0.000 0.000 0.980 0.000 0.020
#&gt; GSM11263     3  0.0162      0.926 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11267     3  0.0162      0.926 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28744     4  0.0324      0.856 0.004 0.000 0.000 0.992 0.004
#&gt; GSM28734     4  0.0613      0.852 0.004 0.000 0.008 0.984 0.004
#&gt; GSM28747     1  0.1831      0.687 0.920 0.000 0.000 0.004 0.076
#&gt; GSM11257     4  0.2628      0.814 0.028 0.000 0.000 0.884 0.088
#&gt; GSM11252     1  0.3187      0.667 0.864 0.000 0.012 0.036 0.088
#&gt; GSM11264     3  0.0000      0.926 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     5  0.4302     -0.259 0.000 0.000 0.480 0.000 0.520
#&gt; GSM11258     4  0.2685      0.792 0.092 0.000 0.000 0.880 0.028
#&gt; GSM28728     5  0.2806      0.600 0.152 0.000 0.004 0.000 0.844
#&gt; GSM28746     1  0.3521      0.636 0.820 0.000 0.000 0.040 0.140
#&gt; GSM28738     5  0.3555      0.624 0.124 0.000 0.000 0.052 0.824
#&gt; GSM28741     2  0.0771      0.969 0.020 0.976 0.000 0.000 0.004
#&gt; GSM28729     5  0.3612      0.636 0.228 0.000 0.000 0.008 0.764
#&gt; GSM28742     5  0.4467      0.567 0.344 0.000 0.000 0.016 0.640
#&gt; GSM11250     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11245     1  0.4160      0.617 0.816 0.000 0.064 0.036 0.084
#&gt; GSM11246     1  0.2852      0.712 0.828 0.000 0.000 0.000 0.172
#&gt; GSM11261     3  0.1478      0.900 0.000 0.000 0.936 0.000 0.064
#&gt; GSM11248     3  0.1341      0.906 0.000 0.000 0.944 0.000 0.056
#&gt; GSM28732     1  0.2929      0.585 0.820 0.000 0.000 0.000 0.180
#&gt; GSM11255     1  0.2707      0.683 0.860 0.000 0.000 0.008 0.132
#&gt; GSM28731     1  0.3895      0.417 0.680 0.000 0.000 0.000 0.320
#&gt; GSM28727     1  0.1270      0.699 0.948 0.000 0.000 0.000 0.052
#&gt; GSM11251     1  0.2280      0.719 0.880 0.000 0.000 0.000 0.120
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.6999     0.3005 0.256 0.000 0.000 0.172 0.460 0.112
#&gt; GSM28736     5  0.7272     0.0449 0.092 0.108 0.000 0.356 0.412 0.032
#&gt; GSM28737     1  0.1572     0.7490 0.936 0.000 0.000 0.000 0.028 0.036
#&gt; GSM11249     3  0.3079     0.6947 0.004 0.000 0.844 0.000 0.056 0.096
#&gt; GSM28745     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.1471     0.7403 0.932 0.000 0.000 0.000 0.004 0.064
#&gt; GSM28739     1  0.2320     0.6847 0.864 0.000 0.000 0.000 0.004 0.132
#&gt; GSM11243     3  0.4555     0.2396 0.028 0.000 0.548 0.000 0.004 0.420
#&gt; GSM28740     1  0.1327     0.7429 0.936 0.000 0.000 0.000 0.000 0.064
#&gt; GSM11259     5  0.6018     0.1164 0.256 0.000 0.000 0.000 0.420 0.324
#&gt; GSM28726     5  0.4319     0.3779 0.168 0.000 0.000 0.000 0.724 0.108
#&gt; GSM28743     1  0.1391     0.7517 0.944 0.000 0.000 0.000 0.016 0.040
#&gt; GSM11256     4  0.0000     0.8582 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11262     1  0.1760     0.7480 0.928 0.000 0.000 0.004 0.020 0.048
#&gt; GSM28724     6  0.5907     0.3053 0.236 0.000 0.016 0.000 0.200 0.548
#&gt; GSM28725     3  0.1387     0.7582 0.000 0.000 0.932 0.000 0.000 0.068
#&gt; GSM11263     3  0.0865     0.7694 0.000 0.000 0.964 0.000 0.000 0.036
#&gt; GSM11267     3  0.0291     0.7720 0.000 0.000 0.992 0.000 0.004 0.004
#&gt; GSM28744     4  0.0000     0.8582 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28734     4  0.0260     0.8568 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28747     5  0.4594    -0.0246 0.480 0.000 0.000 0.000 0.484 0.036
#&gt; GSM11257     4  0.4361     0.5949 0.016 0.000 0.000 0.692 0.260 0.032
#&gt; GSM11252     5  0.6440     0.1524 0.396 0.000 0.060 0.000 0.424 0.120
#&gt; GSM11264     3  0.0603     0.7724 0.000 0.000 0.980 0.000 0.016 0.004
#&gt; GSM11247     6  0.5223    -0.0485 0.052 0.000 0.364 0.000 0.024 0.560
#&gt; GSM11258     4  0.3017     0.7170 0.164 0.000 0.000 0.816 0.000 0.020
#&gt; GSM28728     6  0.5036     0.3176 0.140 0.000 0.000 0.000 0.228 0.632
#&gt; GSM28746     1  0.5717    -0.0102 0.492 0.000 0.000 0.012 0.376 0.120
#&gt; GSM28738     5  0.5377    -0.2027 0.084 0.000 0.000 0.008 0.464 0.444
#&gt; GSM28741     2  0.1753     0.8903 0.000 0.912 0.000 0.000 0.084 0.004
#&gt; GSM28729     5  0.4953    -0.0345 0.056 0.000 0.008 0.000 0.572 0.364
#&gt; GSM28742     5  0.2912     0.2802 0.040 0.000 0.000 0.000 0.844 0.116
#&gt; GSM11250     2  0.0000     0.9898 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11245     5  0.6982     0.2099 0.324 0.000 0.128 0.000 0.424 0.124
#&gt; GSM11246     1  0.1049     0.7523 0.960 0.000 0.000 0.000 0.032 0.008
#&gt; GSM11261     3  0.4525     0.4853 0.008 0.000 0.664 0.004 0.036 0.288
#&gt; GSM11248     3  0.4140     0.6020 0.000 0.000 0.744 0.000 0.152 0.104
#&gt; GSM28732     5  0.4597     0.3527 0.276 0.000 0.000 0.000 0.652 0.072
#&gt; GSM11255     5  0.6271     0.1973 0.384 0.000 0.036 0.000 0.440 0.140
#&gt; GSM28731     5  0.5940     0.2954 0.332 0.000 0.000 0.000 0.440 0.228
#&gt; GSM28727     1  0.4283     0.2019 0.592 0.000 0.000 0.000 0.384 0.024
#&gt; GSM11251     1  0.3287     0.5598 0.768 0.000 0.000 0.000 0.220 0.012
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> SD:NMF 50     0.394 2
#> SD:NMF 49     0.368 3
#> SD:NMF 49     0.464 4
#> SD:NMF 43     0.436 5
#> SD:NMF 30     0.445 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.961       0.982          0.334 0.673   0.673
#> 3 3 0.690           0.847       0.899          0.726 0.740   0.613
#> 4 4 0.728           0.852       0.936          0.163 0.890   0.739
#> 5 5 0.778           0.786       0.869          0.103 0.971   0.908
#> 6 6 0.763           0.712       0.838          0.039 0.969   0.896
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.0376      0.981 0.996 0.004
#&gt; GSM28736     1  0.0376      0.981 0.996 0.004
#&gt; GSM28737     1  0.0000      0.984 1.000 0.000
#&gt; GSM11249     1  0.0938      0.979 0.988 0.012
#&gt; GSM28745     2  0.0938      0.979 0.012 0.988
#&gt; GSM11244     2  0.0938      0.979 0.012 0.988
#&gt; GSM28748     2  0.0938      0.979 0.012 0.988
#&gt; GSM11266     2  0.0938      0.979 0.012 0.988
#&gt; GSM28730     2  0.0938      0.979 0.012 0.988
#&gt; GSM11253     2  0.0938      0.979 0.012 0.988
#&gt; GSM11254     2  0.0938      0.979 0.012 0.988
#&gt; GSM11260     2  0.0938      0.979 0.012 0.988
#&gt; GSM28733     2  0.0938      0.979 0.012 0.988
#&gt; GSM11265     1  0.0000      0.984 1.000 0.000
#&gt; GSM28739     1  0.0000      0.984 1.000 0.000
#&gt; GSM11243     1  0.0938      0.979 0.988 0.012
#&gt; GSM28740     1  0.0000      0.984 1.000 0.000
#&gt; GSM11259     1  0.0000      0.984 1.000 0.000
#&gt; GSM28726     1  0.0000      0.984 1.000 0.000
#&gt; GSM28743     1  0.0000      0.984 1.000 0.000
#&gt; GSM11256     1  0.0000      0.984 1.000 0.000
#&gt; GSM11262     1  0.0000      0.984 1.000 0.000
#&gt; GSM28724     1  0.0000      0.984 1.000 0.000
#&gt; GSM28725     1  0.0938      0.979 0.988 0.012
#&gt; GSM11263     1  0.0938      0.979 0.988 0.012
#&gt; GSM11267     1  0.0938      0.979 0.988 0.012
#&gt; GSM28744     1  0.0000      0.984 1.000 0.000
#&gt; GSM28734     1  0.0000      0.984 1.000 0.000
#&gt; GSM28747     1  0.0000      0.984 1.000 0.000
#&gt; GSM11257     1  0.0000      0.984 1.000 0.000
#&gt; GSM11252     1  0.0938      0.979 0.988 0.012
#&gt; GSM11264     1  0.0938      0.979 0.988 0.012
#&gt; GSM11247     1  0.0938      0.979 0.988 0.012
#&gt; GSM11258     1  0.0000      0.984 1.000 0.000
#&gt; GSM28728     1  0.0000      0.984 1.000 0.000
#&gt; GSM28746     1  0.0000      0.984 1.000 0.000
#&gt; GSM28738     1  0.0000      0.984 1.000 0.000
#&gt; GSM28741     1  0.9896      0.167 0.560 0.440
#&gt; GSM28729     1  0.0000      0.984 1.000 0.000
#&gt; GSM28742     1  0.0000      0.984 1.000 0.000
#&gt; GSM11250     2  0.7056      0.771 0.192 0.808
#&gt; GSM11245     1  0.0938      0.979 0.988 0.012
#&gt; GSM11246     1  0.0000      0.984 1.000 0.000
#&gt; GSM11261     1  0.2236      0.960 0.964 0.036
#&gt; GSM11248     1  0.0938      0.979 0.988 0.012
#&gt; GSM28732     1  0.0000      0.984 1.000 0.000
#&gt; GSM11255     1  0.0000      0.984 1.000 0.000
#&gt; GSM28731     1  0.0000      0.984 1.000 0.000
#&gt; GSM28727     1  0.0000      0.984 1.000 0.000
#&gt; GSM11251     1  0.0000      0.984 1.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0237      0.903 0.996 0.004 0.000
#&gt; GSM28736     1  0.0237      0.903 0.996 0.004 0.000
#&gt; GSM28737     1  0.2537      0.873 0.920 0.000 0.080
#&gt; GSM11249     3  0.3267      0.892 0.116 0.000 0.884
#&gt; GSM28745     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11244     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28748     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11266     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28730     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11253     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11254     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11260     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM28733     2  0.0000      0.972 0.000 1.000 0.000
#&gt; GSM11265     1  0.2537      0.873 0.920 0.000 0.080
#&gt; GSM28739     1  0.2537      0.873 0.920 0.000 0.080
#&gt; GSM11243     3  0.2878      0.896 0.096 0.000 0.904
#&gt; GSM28740     1  0.2537      0.873 0.920 0.000 0.080
#&gt; GSM11259     1  0.0000      0.904 1.000 0.000 0.000
#&gt; GSM28726     1  0.0000      0.904 1.000 0.000 0.000
#&gt; GSM28743     1  0.2537      0.873 0.920 0.000 0.080
#&gt; GSM11256     1  0.2878      0.834 0.904 0.000 0.096
#&gt; GSM11262     1  0.2537      0.873 0.920 0.000 0.080
#&gt; GSM28724     1  0.0000      0.904 1.000 0.000 0.000
#&gt; GSM28725     3  0.2878      0.896 0.096 0.000 0.904
#&gt; GSM11263     3  0.2878      0.896 0.096 0.000 0.904
#&gt; GSM11267     3  0.2878      0.896 0.096 0.000 0.904
#&gt; GSM28744     1  0.2878      0.834 0.904 0.000 0.096
#&gt; GSM28734     1  0.2878      0.834 0.904 0.000 0.096
#&gt; GSM28747     1  0.0000      0.904 1.000 0.000 0.000
#&gt; GSM11257     1  0.1163      0.898 0.972 0.000 0.028
#&gt; GSM11252     3  0.5859      0.653 0.344 0.000 0.656
#&gt; GSM11264     3  0.2878      0.896 0.096 0.000 0.904
#&gt; GSM11247     3  0.2878      0.896 0.096 0.000 0.904
#&gt; GSM11258     1  0.2878      0.834 0.904 0.000 0.096
#&gt; GSM28728     1  0.0237      0.903 0.996 0.000 0.004
#&gt; GSM28746     1  0.4346      0.740 0.816 0.000 0.184
#&gt; GSM28738     1  0.0237      0.903 0.996 0.000 0.004
#&gt; GSM28741     1  0.6267      0.130 0.548 0.452 0.000
#&gt; GSM28729     1  0.1411      0.895 0.964 0.000 0.036
#&gt; GSM28742     1  0.0000      0.904 1.000 0.000 0.000
#&gt; GSM11250     2  0.4291      0.717 0.180 0.820 0.000
#&gt; GSM11245     3  0.5859      0.653 0.344 0.000 0.656
#&gt; GSM11246     1  0.2448      0.875 0.924 0.000 0.076
#&gt; GSM11261     3  0.6512      0.709 0.300 0.024 0.676
#&gt; GSM11248     3  0.3267      0.892 0.116 0.000 0.884
#&gt; GSM28732     1  0.0000      0.904 1.000 0.000 0.000
#&gt; GSM11255     1  0.6286     -0.121 0.536 0.000 0.464
#&gt; GSM28731     1  0.1031      0.899 0.976 0.000 0.024
#&gt; GSM28727     1  0.0000      0.904 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.904 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.0188     0.9110 0.996 0.004 0.000 0.000
#&gt; GSM28736     1  0.0188     0.9110 0.996 0.004 0.000 0.000
#&gt; GSM28737     1  0.2773     0.8698 0.880 0.000 0.116 0.004
#&gt; GSM11249     3  0.1209     0.8143 0.032 0.000 0.964 0.004
#&gt; GSM28745     2  0.0000     0.9679 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9679 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9679 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9679 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9679 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9679 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9679 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9679 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9679 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.2773     0.8698 0.880 0.000 0.116 0.004
#&gt; GSM28739     1  0.2773     0.8698 0.880 0.000 0.116 0.004
#&gt; GSM11243     3  0.0000     0.8183 0.000 0.000 1.000 0.000
#&gt; GSM28740     1  0.2773     0.8698 0.880 0.000 0.116 0.004
#&gt; GSM11259     1  0.0000     0.9128 1.000 0.000 0.000 0.000
#&gt; GSM28726     1  0.0000     0.9128 1.000 0.000 0.000 0.000
#&gt; GSM28743     1  0.2773     0.8698 0.880 0.000 0.116 0.004
#&gt; GSM11256     4  0.0000     1.0000 0.000 0.000 0.000 1.000
#&gt; GSM11262     1  0.2773     0.8698 0.880 0.000 0.116 0.004
#&gt; GSM28724     1  0.0000     0.9128 1.000 0.000 0.000 0.000
#&gt; GSM28725     3  0.0000     0.8183 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000     0.8183 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000     0.8183 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.0000     1.0000 0.000 0.000 0.000 1.000
#&gt; GSM28734     4  0.0000     1.0000 0.000 0.000 0.000 1.000
#&gt; GSM28747     1  0.0000     0.9128 1.000 0.000 0.000 0.000
#&gt; GSM11257     1  0.1118     0.9044 0.964 0.000 0.036 0.000
#&gt; GSM11252     3  0.4313     0.6718 0.260 0.000 0.736 0.004
#&gt; GSM11264     3  0.0000     0.8183 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0000     0.8183 0.000 0.000 1.000 0.000
#&gt; GSM11258     4  0.0000     1.0000 0.000 0.000 0.000 1.000
#&gt; GSM28728     1  0.0188     0.9129 0.996 0.000 0.004 0.000
#&gt; GSM28746     1  0.4155     0.6892 0.756 0.000 0.240 0.004
#&gt; GSM28738     1  0.0188     0.9129 0.996 0.000 0.004 0.000
#&gt; GSM28741     1  0.4967     0.0945 0.548 0.452 0.000 0.000
#&gt; GSM28729     1  0.1557     0.9000 0.944 0.000 0.056 0.000
#&gt; GSM28742     1  0.0000     0.9128 1.000 0.000 0.000 0.000
#&gt; GSM11250     2  0.3400     0.6923 0.180 0.820 0.000 0.000
#&gt; GSM11245     3  0.4313     0.6718 0.260 0.000 0.736 0.004
#&gt; GSM11246     1  0.2714     0.8722 0.884 0.000 0.112 0.004
#&gt; GSM11261     3  0.4607     0.6988 0.204 0.024 0.768 0.004
#&gt; GSM11248     3  0.1209     0.8143 0.032 0.000 0.964 0.004
#&gt; GSM28732     1  0.0000     0.9128 1.000 0.000 0.000 0.000
#&gt; GSM11255     3  0.5165     0.0824 0.484 0.000 0.512 0.004
#&gt; GSM28731     1  0.1118     0.9068 0.964 0.000 0.036 0.000
#&gt; GSM28727     1  0.0000     0.9128 1.000 0.000 0.000 0.000
#&gt; GSM11251     1  0.0000     0.9128 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3 p4    p5
#&gt; GSM28735     1  0.1792      0.756 0.916 0.000 0.000  0 0.084
#&gt; GSM28736     1  0.1792      0.756 0.916 0.000 0.000  0 0.084
#&gt; GSM28737     1  0.4135      0.615 0.656 0.000 0.004  0 0.340
#&gt; GSM11249     5  0.3913      0.586 0.000 0.000 0.324  0 0.676
#&gt; GSM28745     2  0.0000      0.973 0.000 1.000 0.000  0 0.000
#&gt; GSM11244     2  0.0000      0.973 0.000 1.000 0.000  0 0.000
#&gt; GSM28748     2  0.0000      0.973 0.000 1.000 0.000  0 0.000
#&gt; GSM11266     2  0.0000      0.973 0.000 1.000 0.000  0 0.000
#&gt; GSM28730     2  0.0000      0.973 0.000 1.000 0.000  0 0.000
#&gt; GSM11253     2  0.0000      0.973 0.000 1.000 0.000  0 0.000
#&gt; GSM11254     2  0.0000      0.973 0.000 1.000 0.000  0 0.000
#&gt; GSM11260     2  0.0000      0.973 0.000 1.000 0.000  0 0.000
#&gt; GSM28733     2  0.0000      0.973 0.000 1.000 0.000  0 0.000
#&gt; GSM11265     1  0.4135      0.615 0.656 0.000 0.004  0 0.340
#&gt; GSM28739     1  0.4135      0.615 0.656 0.000 0.004  0 0.340
#&gt; GSM11243     3  0.0609      0.982 0.000 0.000 0.980  0 0.020
#&gt; GSM28740     1  0.4135      0.615 0.656 0.000 0.004  0 0.340
#&gt; GSM11259     1  0.0609      0.785 0.980 0.000 0.000  0 0.020
#&gt; GSM28726     1  0.1732      0.760 0.920 0.000 0.000  0 0.080
#&gt; GSM28743     1  0.4135      0.615 0.656 0.000 0.004  0 0.340
#&gt; GSM11256     4  0.0000      1.000 0.000 0.000 0.000  1 0.000
#&gt; GSM11262     1  0.4135      0.615 0.656 0.000 0.004  0 0.340
#&gt; GSM28724     1  0.0404      0.786 0.988 0.000 0.000  0 0.012
#&gt; GSM28725     3  0.0000      0.991 0.000 0.000 1.000  0 0.000
#&gt; GSM11263     3  0.0000      0.991 0.000 0.000 1.000  0 0.000
#&gt; GSM11267     3  0.0000      0.991 0.000 0.000 1.000  0 0.000
#&gt; GSM28744     4  0.0000      1.000 0.000 0.000 0.000  1 0.000
#&gt; GSM28734     4  0.0000      1.000 0.000 0.000 0.000  1 0.000
#&gt; GSM28747     1  0.0880      0.785 0.968 0.000 0.000  0 0.032
#&gt; GSM11257     1  0.2358      0.761 0.888 0.000 0.008  0 0.104
#&gt; GSM11252     5  0.3336      0.739 0.060 0.000 0.096  0 0.844
#&gt; GSM11264     3  0.0000      0.991 0.000 0.000 1.000  0 0.000
#&gt; GSM11247     3  0.0609      0.982 0.000 0.000 0.980  0 0.020
#&gt; GSM11258     4  0.0000      1.000 0.000 0.000 0.000  1 0.000
#&gt; GSM28728     1  0.0404      0.785 0.988 0.000 0.000  0 0.012
#&gt; GSM28746     1  0.5143      0.394 0.532 0.000 0.040  0 0.428
#&gt; GSM28738     1  0.1965      0.761 0.904 0.000 0.000  0 0.096
#&gt; GSM28741     1  0.5736     -0.101 0.468 0.448 0.000  0 0.084
#&gt; GSM28729     1  0.2424      0.761 0.868 0.000 0.000  0 0.132
#&gt; GSM28742     1  0.1732      0.760 0.920 0.000 0.000  0 0.080
#&gt; GSM11250     2  0.3171      0.733 0.176 0.816 0.000  0 0.008
#&gt; GSM11245     5  0.3336      0.739 0.060 0.000 0.096  0 0.844
#&gt; GSM11246     1  0.4101      0.622 0.664 0.000 0.004  0 0.332
#&gt; GSM11261     5  0.4185      0.622 0.024 0.008 0.216  0 0.752
#&gt; GSM11248     5  0.3913      0.586 0.000 0.000 0.324  0 0.676
#&gt; GSM28732     1  0.0794      0.785 0.972 0.000 0.000  0 0.028
#&gt; GSM11255     5  0.4350      0.431 0.268 0.000 0.028  0 0.704
#&gt; GSM28731     1  0.1732      0.774 0.920 0.000 0.000  0 0.080
#&gt; GSM28727     1  0.1043      0.785 0.960 0.000 0.000  0 0.040
#&gt; GSM11251     1  0.1043      0.785 0.960 0.000 0.000  0 0.040
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3 p4    p5    p6
#&gt; GSM28735     1  0.2912      0.683 0.816 0.000 0.000  0 0.172 0.012
#&gt; GSM28736     1  0.2912      0.683 0.816 0.000 0.000  0 0.172 0.012
#&gt; GSM28737     1  0.4915      0.608 0.652 0.000 0.000  0 0.140 0.208
#&gt; GSM11249     6  0.3201      0.358 0.000 0.000 0.208  0 0.012 0.780
#&gt; GSM28745     2  0.0000      0.903 0.000 1.000 0.000  0 0.000 0.000
#&gt; GSM11244     2  0.0000      0.903 0.000 1.000 0.000  0 0.000 0.000
#&gt; GSM28748     2  0.0000      0.903 0.000 1.000 0.000  0 0.000 0.000
#&gt; GSM11266     2  0.0000      0.903 0.000 1.000 0.000  0 0.000 0.000
#&gt; GSM28730     2  0.0000      0.903 0.000 1.000 0.000  0 0.000 0.000
#&gt; GSM11253     2  0.0000      0.903 0.000 1.000 0.000  0 0.000 0.000
#&gt; GSM11254     2  0.0000      0.903 0.000 1.000 0.000  0 0.000 0.000
#&gt; GSM11260     2  0.0000      0.903 0.000 1.000 0.000  0 0.000 0.000
#&gt; GSM28733     2  0.0000      0.903 0.000 1.000 0.000  0 0.000 0.000
#&gt; GSM11265     1  0.4915      0.608 0.652 0.000 0.000  0 0.140 0.208
#&gt; GSM28739     1  0.4915      0.608 0.652 0.000 0.000  0 0.140 0.208
#&gt; GSM11243     3  0.0972      0.961 0.000 0.000 0.964  0 0.028 0.008
#&gt; GSM28740     1  0.4915      0.608 0.652 0.000 0.000  0 0.140 0.208
#&gt; GSM11259     1  0.0146      0.751 0.996 0.000 0.000  0 0.004 0.000
#&gt; GSM28726     1  0.2527      0.692 0.832 0.000 0.000  0 0.168 0.000
#&gt; GSM28743     1  0.4915      0.608 0.652 0.000 0.000  0 0.140 0.208
#&gt; GSM11256     4  0.0000      1.000 0.000 0.000 0.000  1 0.000 0.000
#&gt; GSM11262     1  0.4915      0.608 0.652 0.000 0.000  0 0.140 0.208
#&gt; GSM28724     1  0.0935      0.751 0.964 0.000 0.000  0 0.032 0.004
#&gt; GSM28725     3  0.0000      0.981 0.000 0.000 1.000  0 0.000 0.000
#&gt; GSM11263     3  0.0000      0.981 0.000 0.000 1.000  0 0.000 0.000
#&gt; GSM11267     3  0.0000      0.981 0.000 0.000 1.000  0 0.000 0.000
#&gt; GSM28744     4  0.0000      1.000 0.000 0.000 0.000  1 0.000 0.000
#&gt; GSM28734     4  0.0000      1.000 0.000 0.000 0.000  1 0.000 0.000
#&gt; GSM28747     1  0.0547      0.750 0.980 0.000 0.000  0 0.020 0.000
#&gt; GSM11257     1  0.4461      0.411 0.564 0.000 0.000  0 0.404 0.032
#&gt; GSM11252     6  0.1327      0.501 0.064 0.000 0.000  0 0.000 0.936
#&gt; GSM11264     3  0.0000      0.981 0.000 0.000 1.000  0 0.000 0.000
#&gt; GSM11247     3  0.0972      0.961 0.000 0.000 0.964  0 0.028 0.008
#&gt; GSM11258     4  0.0000      1.000 0.000 0.000 0.000  1 0.000 0.000
#&gt; GSM28728     1  0.1204      0.752 0.944 0.000 0.000  0 0.056 0.000
#&gt; GSM28746     1  0.4660      0.355 0.540 0.000 0.000  0 0.044 0.416
#&gt; GSM28738     1  0.3833      0.411 0.556 0.000 0.000  0 0.444 0.000
#&gt; GSM28741     2  0.5886      0.123 0.400 0.448 0.000  0 0.140 0.012
#&gt; GSM28729     1  0.2883      0.726 0.788 0.000 0.000  0 0.212 0.000
#&gt; GSM28742     1  0.2527      0.692 0.832 0.000 0.000  0 0.168 0.000
#&gt; GSM11250     2  0.3300      0.695 0.148 0.816 0.000  0 0.024 0.012
#&gt; GSM11245     6  0.1327      0.501 0.064 0.000 0.000  0 0.000 0.936
#&gt; GSM11246     1  0.4855      0.613 0.660 0.000 0.000  0 0.136 0.204
#&gt; GSM11261     5  0.6340      0.000 0.024 0.000 0.188  0 0.420 0.368
#&gt; GSM11248     6  0.3201      0.358 0.000 0.000 0.208  0 0.012 0.780
#&gt; GSM28732     1  0.0146      0.752 0.996 0.000 0.000  0 0.004 0.000
#&gt; GSM11255     6  0.5103      0.171 0.268 0.000 0.000  0 0.124 0.608
#&gt; GSM28731     1  0.1501      0.742 0.924 0.000 0.000  0 0.076 0.000
#&gt; GSM28727     1  0.0547      0.750 0.980 0.000 0.000  0 0.020 0.000
#&gt; GSM11251     1  0.0547      0.750 0.980 0.000 0.000  0 0.020 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:hclust 49     0.393 2
#> CV:hclust 48     0.367 3
#> CV:hclust 48     0.509 4
#> CV:hclust 47     0.464 5
#> CV:hclust 42     0.448 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:kmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.362           0.843       0.823         0.3625 0.650   0.650
#> 3 3 0.944           0.959       0.961         0.4982 0.764   0.651
#> 4 4 0.716           0.790       0.866         0.2598 0.909   0.803
#> 5 5 0.772           0.804       0.883         0.1189 0.853   0.606
#> 6 6 0.793           0.717       0.819         0.0629 0.939   0.756
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.6048      0.848 0.852 0.148
#&gt; GSM28736     1  0.6048      0.848 0.852 0.148
#&gt; GSM28737     1  0.6048      0.848 0.852 0.148
#&gt; GSM11249     1  0.6438      0.704 0.836 0.164
#&gt; GSM28745     2  0.6438      0.989 0.164 0.836
#&gt; GSM11244     2  0.6438      0.989 0.164 0.836
#&gt; GSM28748     2  0.6438      0.989 0.164 0.836
#&gt; GSM11266     2  0.6438      0.989 0.164 0.836
#&gt; GSM28730     2  0.6438      0.989 0.164 0.836
#&gt; GSM11253     2  0.6438      0.989 0.164 0.836
#&gt; GSM11254     2  0.6438      0.989 0.164 0.836
#&gt; GSM11260     2  0.6438      0.989 0.164 0.836
#&gt; GSM28733     2  0.6438      0.989 0.164 0.836
#&gt; GSM11265     1  0.6048      0.848 0.852 0.148
#&gt; GSM28739     1  0.6048      0.848 0.852 0.148
#&gt; GSM11243     1  0.7674      0.664 0.776 0.224
#&gt; GSM28740     1  0.6048      0.848 0.852 0.148
#&gt; GSM11259     1  0.6048      0.848 0.852 0.148
#&gt; GSM28726     1  0.6048      0.848 0.852 0.148
#&gt; GSM28743     1  0.6048      0.848 0.852 0.148
#&gt; GSM11256     1  0.0376      0.818 0.996 0.004
#&gt; GSM11262     1  0.6048      0.848 0.852 0.148
#&gt; GSM28724     1  0.6048      0.848 0.852 0.148
#&gt; GSM28725     1  0.7674      0.664 0.776 0.224
#&gt; GSM11263     1  0.7674      0.664 0.776 0.224
#&gt; GSM11267     1  0.7674      0.664 0.776 0.224
#&gt; GSM28744     1  0.0376      0.818 0.996 0.004
#&gt; GSM28734     1  0.2603      0.795 0.956 0.044
#&gt; GSM28747     1  0.6048      0.848 0.852 0.148
#&gt; GSM11257     1  0.3431      0.835 0.936 0.064
#&gt; GSM11252     1  0.0000      0.820 1.000 0.000
#&gt; GSM11264     1  0.7674      0.664 0.776 0.224
#&gt; GSM11247     1  0.7674      0.664 0.776 0.224
#&gt; GSM11258     1  0.0376      0.818 0.996 0.004
#&gt; GSM28728     1  0.6048      0.848 0.852 0.148
#&gt; GSM28746     1  0.1414      0.825 0.980 0.020
#&gt; GSM28738     1  0.6048      0.848 0.852 0.148
#&gt; GSM28741     2  0.7950      0.884 0.240 0.760
#&gt; GSM28729     1  0.6048      0.848 0.852 0.148
#&gt; GSM28742     1  0.6048      0.848 0.852 0.148
#&gt; GSM11250     2  0.6438      0.989 0.164 0.836
#&gt; GSM11245     1  0.0376      0.818 0.996 0.004
#&gt; GSM11246     1  0.6048      0.848 0.852 0.148
#&gt; GSM11261     1  0.4298      0.792 0.912 0.088
#&gt; GSM11248     1  0.6438      0.704 0.836 0.164
#&gt; GSM28732     1  0.6048      0.848 0.852 0.148
#&gt; GSM11255     1  0.0000      0.820 1.000 0.000
#&gt; GSM28731     1  0.6048      0.848 0.852 0.148
#&gt; GSM28727     1  0.6048      0.848 0.852 0.148
#&gt; GSM11251     1  0.6048      0.848 0.852 0.148
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM28736     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM28737     1  0.0237      0.960 0.996 0.000 0.004
#&gt; GSM11249     3  0.0892      1.000 0.020 0.000 0.980
#&gt; GSM28745     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM11244     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM28748     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM11266     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM28730     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM11253     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM11254     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM11260     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM28733     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM11265     1  0.1289      0.956 0.968 0.000 0.032
#&gt; GSM28739     1  0.1289      0.956 0.968 0.000 0.032
#&gt; GSM11243     3  0.0892      1.000 0.020 0.000 0.980
#&gt; GSM28740     1  0.1289      0.956 0.968 0.000 0.032
#&gt; GSM11259     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM28726     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM28743     1  0.1289      0.956 0.968 0.000 0.032
#&gt; GSM11256     1  0.3692      0.910 0.896 0.048 0.056
#&gt; GSM11262     1  0.1289      0.956 0.968 0.000 0.032
#&gt; GSM28724     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM28725     3  0.0892      1.000 0.020 0.000 0.980
#&gt; GSM11263     3  0.0892      1.000 0.020 0.000 0.980
#&gt; GSM11267     3  0.0892      1.000 0.020 0.000 0.980
#&gt; GSM28744     1  0.3589      0.912 0.900 0.048 0.052
#&gt; GSM28734     1  0.5435      0.829 0.808 0.048 0.144
#&gt; GSM28747     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM11257     1  0.1453      0.948 0.968 0.024 0.008
#&gt; GSM11252     1  0.1411      0.953 0.964 0.000 0.036
#&gt; GSM11264     3  0.0892      1.000 0.020 0.000 0.980
#&gt; GSM11247     3  0.0892      1.000 0.020 0.000 0.980
#&gt; GSM11258     1  0.3983      0.908 0.884 0.048 0.068
#&gt; GSM28728     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM28746     1  0.1529      0.951 0.960 0.000 0.040
#&gt; GSM28738     1  0.1015      0.954 0.980 0.012 0.008
#&gt; GSM28741     1  0.3267      0.861 0.884 0.116 0.000
#&gt; GSM28729     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM28742     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM11250     2  0.1753      1.000 0.048 0.952 0.000
#&gt; GSM11245     1  0.1529      0.951 0.960 0.000 0.040
#&gt; GSM11246     1  0.0747      0.959 0.984 0.000 0.016
#&gt; GSM11261     1  0.5497      0.635 0.708 0.000 0.292
#&gt; GSM11248     3  0.0892      1.000 0.020 0.000 0.980
#&gt; GSM28732     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM11255     1  0.1529      0.951 0.960 0.000 0.040
#&gt; GSM28731     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.961 1.000 0.000 0.000
#&gt; GSM11251     1  0.0237      0.960 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.2011      0.727 0.920 0.000 0.000 0.080
#&gt; GSM28736     1  0.3311      0.666 0.828 0.000 0.000 0.172
#&gt; GSM28737     1  0.4401      0.659 0.724 0.000 0.004 0.272
#&gt; GSM11249     3  0.2164      0.932 0.004 0.004 0.924 0.068
#&gt; GSM28745     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11244     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM28748     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11266     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM28730     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11253     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11254     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11260     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM28733     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11265     1  0.4608      0.634 0.692 0.000 0.004 0.304
#&gt; GSM28739     1  0.4608      0.634 0.692 0.000 0.004 0.304
#&gt; GSM11243     3  0.0895      0.968 0.004 0.000 0.976 0.020
#&gt; GSM28740     1  0.4608      0.634 0.692 0.000 0.004 0.304
#&gt; GSM11259     1  0.0336      0.753 0.992 0.000 0.000 0.008
#&gt; GSM28726     1  0.3311      0.666 0.828 0.000 0.000 0.172
#&gt; GSM28743     1  0.4608      0.634 0.692 0.000 0.004 0.304
#&gt; GSM11256     4  0.3626      0.743 0.184 0.000 0.004 0.812
#&gt; GSM11262     1  0.4608      0.634 0.692 0.000 0.004 0.304
#&gt; GSM28724     1  0.0336      0.753 0.992 0.000 0.000 0.008
#&gt; GSM28725     3  0.0188      0.973 0.004 0.000 0.996 0.000
#&gt; GSM11263     3  0.0188      0.973 0.004 0.000 0.996 0.000
#&gt; GSM11267     3  0.0376      0.973 0.004 0.004 0.992 0.000
#&gt; GSM28744     4  0.1978      0.862 0.068 0.000 0.004 0.928
#&gt; GSM28734     4  0.1833      0.850 0.024 0.000 0.032 0.944
#&gt; GSM28747     1  0.0921      0.754 0.972 0.000 0.000 0.028
#&gt; GSM11257     1  0.4122      0.585 0.760 0.004 0.000 0.236
#&gt; GSM11252     1  0.4401      0.640 0.724 0.000 0.004 0.272
#&gt; GSM11264     3  0.0376      0.973 0.004 0.004 0.992 0.000
#&gt; GSM11247     3  0.0895      0.968 0.004 0.000 0.976 0.020
#&gt; GSM11258     4  0.2345      0.785 0.100 0.000 0.000 0.900
#&gt; GSM28728     1  0.1716      0.735 0.936 0.000 0.000 0.064
#&gt; GSM28746     1  0.3208      0.722 0.848 0.000 0.004 0.148
#&gt; GSM28738     1  0.4018      0.598 0.772 0.004 0.000 0.224
#&gt; GSM28741     1  0.3198      0.701 0.880 0.080 0.000 0.040
#&gt; GSM28729     1  0.3024      0.686 0.852 0.000 0.000 0.148
#&gt; GSM28742     1  0.3311      0.666 0.828 0.000 0.000 0.172
#&gt; GSM11250     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11245     1  0.4655      0.602 0.684 0.000 0.004 0.312
#&gt; GSM11246     1  0.4584      0.637 0.696 0.000 0.004 0.300
#&gt; GSM11261     1  0.6627      0.144 0.504 0.000 0.412 0.084
#&gt; GSM11248     3  0.2088      0.936 0.004 0.004 0.928 0.064
#&gt; GSM28732     1  0.0188      0.754 0.996 0.000 0.000 0.004
#&gt; GSM11255     1  0.4313      0.664 0.736 0.000 0.004 0.260
#&gt; GSM28731     1  0.0817      0.754 0.976 0.000 0.000 0.024
#&gt; GSM28727     1  0.0000      0.754 1.000 0.000 0.000 0.000
#&gt; GSM11251     1  0.0188      0.754 0.996 0.000 0.004 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.1195     0.7849 0.028 0.000 0.000 0.012 0.960
#&gt; GSM28736     5  0.0771     0.7802 0.004 0.000 0.000 0.020 0.976
#&gt; GSM28737     1  0.3093     0.8858 0.824 0.000 0.000 0.008 0.168
#&gt; GSM11249     3  0.3612     0.8399 0.100 0.000 0.832 0.064 0.004
#&gt; GSM28745     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3123     0.8901 0.828 0.000 0.000 0.012 0.160
#&gt; GSM28739     1  0.3123     0.8901 0.828 0.000 0.000 0.012 0.160
#&gt; GSM11243     3  0.1907     0.9099 0.044 0.000 0.928 0.028 0.000
#&gt; GSM28740     1  0.3123     0.8901 0.828 0.000 0.000 0.012 0.160
#&gt; GSM11259     5  0.1732     0.7735 0.080 0.000 0.000 0.000 0.920
#&gt; GSM28726     5  0.0771     0.7802 0.004 0.000 0.000 0.020 0.976
#&gt; GSM28743     1  0.3123     0.8901 0.828 0.000 0.000 0.012 0.160
#&gt; GSM11256     4  0.1211     0.9800 0.016 0.000 0.000 0.960 0.024
#&gt; GSM11262     1  0.3123     0.8901 0.828 0.000 0.000 0.012 0.160
#&gt; GSM28724     5  0.2011     0.7747 0.088 0.000 0.000 0.004 0.908
#&gt; GSM28725     3  0.0000     0.9313 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0404     0.9300 0.012 0.000 0.988 0.000 0.000
#&gt; GSM11267     3  0.0162     0.9312 0.000 0.000 0.996 0.004 0.000
#&gt; GSM28744     4  0.1211     0.9889 0.024 0.000 0.000 0.960 0.016
#&gt; GSM28734     4  0.1211     0.9889 0.024 0.000 0.000 0.960 0.016
#&gt; GSM28747     5  0.4227     0.1409 0.420 0.000 0.000 0.000 0.580
#&gt; GSM11257     5  0.1522     0.7581 0.012 0.000 0.000 0.044 0.944
#&gt; GSM11252     1  0.4728     0.6548 0.700 0.000 0.000 0.060 0.240
#&gt; GSM11264     3  0.0162     0.9312 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11247     3  0.2139     0.9038 0.052 0.000 0.916 0.032 0.000
#&gt; GSM11258     4  0.1364     0.9790 0.036 0.000 0.000 0.952 0.012
#&gt; GSM28728     5  0.1768     0.7801 0.072 0.000 0.000 0.004 0.924
#&gt; GSM28746     5  0.4446    -0.0207 0.476 0.000 0.000 0.004 0.520
#&gt; GSM28738     5  0.1444     0.7597 0.012 0.000 0.000 0.040 0.948
#&gt; GSM28741     5  0.1455     0.7846 0.032 0.008 0.000 0.008 0.952
#&gt; GSM28729     5  0.0798     0.7775 0.016 0.000 0.000 0.008 0.976
#&gt; GSM28742     5  0.0798     0.7790 0.008 0.000 0.000 0.016 0.976
#&gt; GSM11250     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11245     1  0.4701     0.6608 0.704 0.000 0.000 0.060 0.236
#&gt; GSM11246     1  0.3163     0.8886 0.824 0.000 0.000 0.012 0.164
#&gt; GSM11261     5  0.6536     0.0134 0.088 0.000 0.392 0.036 0.484
#&gt; GSM11248     3  0.3547     0.8437 0.100 0.000 0.836 0.060 0.004
#&gt; GSM28732     5  0.1792     0.7703 0.084 0.000 0.000 0.000 0.916
#&gt; GSM11255     1  0.3109     0.7479 0.800 0.000 0.000 0.000 0.200
#&gt; GSM28731     5  0.4235     0.1423 0.424 0.000 0.000 0.000 0.576
#&gt; GSM28727     5  0.2690     0.7096 0.156 0.000 0.000 0.000 0.844
#&gt; GSM11251     5  0.3837     0.4649 0.308 0.000 0.000 0.000 0.692
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM28735     5  0.0458     0.6830 0.016 0.000 0.000 0.000 0.984 NA
#&gt; GSM28736     5  0.1590     0.6786 0.008 0.000 0.000 0.008 0.936 NA
#&gt; GSM28737     1  0.0458     0.7036 0.984 0.000 0.000 0.000 0.016 NA
#&gt; GSM11249     3  0.3986     0.7496 0.012 0.000 0.748 0.036 0.000 NA
#&gt; GSM28745     2  0.0260     0.9959 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM11244     2  0.0000     0.9972 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM28748     2  0.0146     0.9970 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM11266     2  0.0000     0.9972 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM28730     2  0.0260     0.9959 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM11253     2  0.0146     0.9970 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM11254     2  0.0000     0.9972 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM11260     2  0.0260     0.9959 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM28733     2  0.0000     0.9972 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM11265     1  0.0748     0.7026 0.976 0.000 0.000 0.004 0.016 NA
#&gt; GSM28739     1  0.0748     0.7026 0.976 0.000 0.000 0.004 0.016 NA
#&gt; GSM11243     3  0.2302     0.8473 0.000 0.000 0.872 0.008 0.000 NA
#&gt; GSM28740     1  0.0603     0.7033 0.980 0.000 0.000 0.004 0.016 NA
#&gt; GSM11259     5  0.4535     0.6220 0.152 0.000 0.000 0.000 0.704 NA
#&gt; GSM28726     5  0.3133     0.6486 0.008 0.000 0.000 0.008 0.804 NA
#&gt; GSM28743     1  0.0862     0.7025 0.972 0.000 0.000 0.004 0.016 NA
#&gt; GSM11256     4  0.0725     0.9829 0.012 0.000 0.000 0.976 0.000 NA
#&gt; GSM11262     1  0.0862     0.7025 0.972 0.000 0.000 0.004 0.016 NA
#&gt; GSM28724     5  0.5156     0.5658 0.164 0.000 0.000 0.000 0.620 NA
#&gt; GSM28725     3  0.0000     0.8928 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11263     3  0.0000     0.8928 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11267     3  0.0000     0.8928 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM28744     4  0.0260     0.9856 0.008 0.000 0.000 0.992 0.000 NA
#&gt; GSM28734     4  0.0520     0.9845 0.008 0.000 0.000 0.984 0.000 NA
#&gt; GSM28747     1  0.5954     0.0174 0.408 0.000 0.000 0.000 0.372 NA
#&gt; GSM11257     5  0.4635     0.5559 0.008 0.000 0.000 0.024 0.488 NA
#&gt; GSM11252     1  0.5961     0.4687 0.456 0.000 0.000 0.036 0.096 NA
#&gt; GSM11264     3  0.0000     0.8928 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11247     3  0.2346     0.8452 0.000 0.000 0.868 0.008 0.000 NA
#&gt; GSM11258     4  0.0713     0.9765 0.028 0.000 0.000 0.972 0.000 NA
#&gt; GSM28728     5  0.4261     0.6472 0.112 0.000 0.000 0.000 0.732 NA
#&gt; GSM28746     1  0.6149     0.1944 0.380 0.000 0.000 0.004 0.244 NA
#&gt; GSM28738     5  0.4362     0.5768 0.004 0.000 0.000 0.020 0.584 NA
#&gt; GSM28741     5  0.1074     0.6776 0.012 0.000 0.000 0.000 0.960 NA
#&gt; GSM28729     5  0.4321     0.6229 0.012 0.000 0.000 0.008 0.580 NA
#&gt; GSM28742     5  0.3023     0.6479 0.004 0.000 0.000 0.008 0.808 NA
#&gt; GSM11250     2  0.0000     0.9972 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM11245     1  0.5961     0.4687 0.456 0.000 0.000 0.036 0.096 NA
#&gt; GSM11246     1  0.0458     0.7036 0.984 0.000 0.000 0.000 0.016 NA
#&gt; GSM11261     5  0.6840     0.1910 0.040 0.000 0.252 0.004 0.404 NA
#&gt; GSM11248     3  0.4015     0.7464 0.012 0.000 0.744 0.036 0.000 NA
#&gt; GSM28732     5  0.5170     0.5213 0.176 0.000 0.000 0.000 0.620 NA
#&gt; GSM11255     1  0.4388     0.5438 0.572 0.000 0.000 0.000 0.028 NA
#&gt; GSM28731     1  0.6051    -0.0274 0.384 0.000 0.000 0.000 0.360 NA
#&gt; GSM28727     5  0.4358     0.5862 0.196 0.000 0.000 0.000 0.712 NA
#&gt; GSM11251     5  0.5083     0.3888 0.320 0.000 0.000 0.000 0.580 NA
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:kmeans 50     0.394 2
#> CV:kmeans 50     0.370 3
#> CV:kmeans 49     0.509 4
#> CV:kmeans 45     0.483 5
#> CV:kmeans 43     0.487 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:skmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.735           0.842       0.927         0.4614 0.519   0.519
#> 3 3 0.856           0.838       0.938         0.4334 0.726   0.518
#> 4 4 0.757           0.746       0.878         0.1360 0.776   0.451
#> 5 5 0.802           0.782       0.886         0.0700 0.933   0.738
#> 6 6 0.818           0.705       0.807         0.0389 0.968   0.842
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1   0.224      0.921 0.964 0.036
#&gt; GSM28736     2   0.871      0.523 0.292 0.708
#&gt; GSM28737     1   0.000      0.958 1.000 0.000
#&gt; GSM11249     1   0.000      0.958 1.000 0.000
#&gt; GSM28745     2   0.000      0.836 0.000 1.000
#&gt; GSM11244     2   0.000      0.836 0.000 1.000
#&gt; GSM28748     2   0.000      0.836 0.000 1.000
#&gt; GSM11266     2   0.000      0.836 0.000 1.000
#&gt; GSM28730     2   0.000      0.836 0.000 1.000
#&gt; GSM11253     2   0.000      0.836 0.000 1.000
#&gt; GSM11254     2   0.000      0.836 0.000 1.000
#&gt; GSM11260     2   0.000      0.836 0.000 1.000
#&gt; GSM28733     2   0.000      0.836 0.000 1.000
#&gt; GSM11265     1   0.000      0.958 1.000 0.000
#&gt; GSM28739     1   0.000      0.958 1.000 0.000
#&gt; GSM11243     2   0.955      0.571 0.376 0.624
#&gt; GSM28740     1   0.000      0.958 1.000 0.000
#&gt; GSM11259     1   0.000      0.958 1.000 0.000
#&gt; GSM28726     1   0.958      0.358 0.620 0.380
#&gt; GSM28743     1   0.000      0.958 1.000 0.000
#&gt; GSM11256     1   0.000      0.958 1.000 0.000
#&gt; GSM11262     1   0.000      0.958 1.000 0.000
#&gt; GSM28724     1   0.000      0.958 1.000 0.000
#&gt; GSM28725     2   0.955      0.571 0.376 0.624
#&gt; GSM11263     2   0.955      0.571 0.376 0.624
#&gt; GSM11267     2   0.961      0.556 0.384 0.616
#&gt; GSM28744     1   0.000      0.958 1.000 0.000
#&gt; GSM28734     1   0.000      0.958 1.000 0.000
#&gt; GSM28747     1   0.000      0.958 1.000 0.000
#&gt; GSM11257     1   0.000      0.958 1.000 0.000
#&gt; GSM11252     1   0.000      0.958 1.000 0.000
#&gt; GSM11264     2   0.955      0.571 0.376 0.624
#&gt; GSM11247     2   0.955      0.571 0.376 0.624
#&gt; GSM11258     1   0.000      0.958 1.000 0.000
#&gt; GSM28728     1   0.000      0.958 1.000 0.000
#&gt; GSM28746     1   0.000      0.958 1.000 0.000
#&gt; GSM28738     1   0.973      0.301 0.596 0.404
#&gt; GSM28741     2   0.000      0.836 0.000 1.000
#&gt; GSM28729     1   0.000      0.958 1.000 0.000
#&gt; GSM28742     1   0.730      0.696 0.796 0.204
#&gt; GSM11250     2   0.000      0.836 0.000 1.000
#&gt; GSM11245     1   0.000      0.958 1.000 0.000
#&gt; GSM11246     1   0.000      0.958 1.000 0.000
#&gt; GSM11261     2   0.388      0.805 0.076 0.924
#&gt; GSM11248     1   0.000      0.958 1.000 0.000
#&gt; GSM28732     1   0.000      0.958 1.000 0.000
#&gt; GSM11255     1   0.000      0.958 1.000 0.000
#&gt; GSM28731     1   0.000      0.958 1.000 0.000
#&gt; GSM28727     1   0.000      0.958 1.000 0.000
#&gt; GSM11251     1   0.000      0.958 1.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM28736     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM28737     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM11249     3  0.0000     0.9275 0.000 0.000 1.000
#&gt; GSM28745     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM11244     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM28748     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM11266     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM28730     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM11253     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM11254     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM11260     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM28733     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM11265     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM28739     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM11243     3  0.0000     0.9275 0.000 0.000 1.000
#&gt; GSM28740     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM11259     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM28726     2  0.3551     0.8384 0.132 0.868 0.000
#&gt; GSM28743     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM11256     3  0.0747     0.9207 0.016 0.000 0.984
#&gt; GSM11262     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM28724     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM28725     3  0.0000     0.9275 0.000 0.000 1.000
#&gt; GSM11263     3  0.0000     0.9275 0.000 0.000 1.000
#&gt; GSM11267     3  0.0000     0.9275 0.000 0.000 1.000
#&gt; GSM28744     3  0.1964     0.8898 0.056 0.000 0.944
#&gt; GSM28734     3  0.0747     0.9207 0.016 0.000 0.984
#&gt; GSM28747     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM11257     3  0.5397     0.5787 0.280 0.000 0.720
#&gt; GSM11252     1  0.6180     0.2528 0.584 0.000 0.416
#&gt; GSM11264     3  0.0000     0.9275 0.000 0.000 1.000
#&gt; GSM11247     3  0.0000     0.9275 0.000 0.000 1.000
#&gt; GSM11258     1  0.5948     0.4028 0.640 0.000 0.360
#&gt; GSM28728     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM28746     1  0.4654     0.6777 0.792 0.000 0.208
#&gt; GSM28738     1  0.9857     0.0571 0.404 0.336 0.260
#&gt; GSM28741     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM28729     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM28742     1  0.6299     0.0478 0.524 0.476 0.000
#&gt; GSM11250     2  0.0000     0.9875 0.000 1.000 0.000
#&gt; GSM11245     3  0.6286     0.0752 0.464 0.000 0.536
#&gt; GSM11246     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM11261     3  0.0000     0.9275 0.000 0.000 1.000
#&gt; GSM11248     3  0.0000     0.9275 0.000 0.000 1.000
#&gt; GSM28732     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM11255     1  0.2261     0.8384 0.932 0.000 0.068
#&gt; GSM28731     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000     0.8944 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000     0.8944 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.0817     0.6374 0.976 0.000 0.000 0.024
#&gt; GSM28736     1  0.3837     0.4586 0.776 0.224 0.000 0.000
#&gt; GSM28737     4  0.0188     0.8693 0.004 0.000 0.000 0.996
#&gt; GSM11249     3  0.0000     0.9597 0.000 0.000 1.000 0.000
#&gt; GSM28745     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11265     4  0.0000     0.8723 0.000 0.000 0.000 1.000
#&gt; GSM28739     4  0.0000     0.8723 0.000 0.000 0.000 1.000
#&gt; GSM11243     3  0.0000     0.9597 0.000 0.000 1.000 0.000
#&gt; GSM28740     4  0.0000     0.8723 0.000 0.000 0.000 1.000
#&gt; GSM11259     1  0.4925     0.4281 0.572 0.000 0.000 0.428
#&gt; GSM28726     1  0.0188     0.6377 0.996 0.000 0.000 0.004
#&gt; GSM28743     4  0.0000     0.8723 0.000 0.000 0.000 1.000
#&gt; GSM11256     1  0.6865    -0.0340 0.524 0.000 0.364 0.112
#&gt; GSM11262     4  0.0000     0.8723 0.000 0.000 0.000 1.000
#&gt; GSM28724     1  0.4992     0.3528 0.524 0.000 0.000 0.476
#&gt; GSM28725     3  0.0000     0.9597 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000     0.9597 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000     0.9597 0.000 0.000 1.000 0.000
#&gt; GSM28744     1  0.6602     0.0312 0.484 0.000 0.080 0.436
#&gt; GSM28734     3  0.6719     0.5287 0.204 0.000 0.616 0.180
#&gt; GSM28747     1  0.4998     0.3280 0.512 0.000 0.000 0.488
#&gt; GSM11257     1  0.2060     0.6108 0.932 0.000 0.052 0.016
#&gt; GSM11252     4  0.4776     0.6549 0.016 0.000 0.272 0.712
#&gt; GSM11264     3  0.0000     0.9597 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0000     0.9597 0.000 0.000 1.000 0.000
#&gt; GSM11258     4  0.1022     0.8456 0.032 0.000 0.000 0.968
#&gt; GSM28728     1  0.2814     0.6133 0.868 0.000 0.000 0.132
#&gt; GSM28746     4  0.5613     0.6302 0.120 0.000 0.156 0.724
#&gt; GSM28738     1  0.0188     0.6355 0.996 0.004 0.000 0.000
#&gt; GSM28741     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28729     1  0.0188     0.6377 0.996 0.000 0.000 0.004
#&gt; GSM28742     1  0.0188     0.6377 0.996 0.000 0.000 0.004
#&gt; GSM11250     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11245     4  0.4819     0.5631 0.004 0.000 0.344 0.652
#&gt; GSM11246     4  0.0000     0.8723 0.000 0.000 0.000 1.000
#&gt; GSM11261     3  0.0000     0.9597 0.000 0.000 1.000 0.000
#&gt; GSM11248     3  0.0000     0.9597 0.000 0.000 1.000 0.000
#&gt; GSM28732     1  0.4941     0.4197 0.564 0.000 0.000 0.436
#&gt; GSM11255     4  0.2944     0.7943 0.004 0.000 0.128 0.868
#&gt; GSM28731     1  0.4994     0.3462 0.520 0.000 0.000 0.480
#&gt; GSM28727     1  0.4955     0.4095 0.556 0.000 0.000 0.444
#&gt; GSM11251     1  0.4955     0.4095 0.556 0.000 0.000 0.444
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.2773     0.6905 0.020 0.000 0.000 0.112 0.868
#&gt; GSM28736     5  0.5413     0.4875 0.000 0.172 0.000 0.164 0.664
#&gt; GSM28737     1  0.0162     0.8518 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11249     3  0.2439     0.8447 0.000 0.000 0.876 0.120 0.004
#&gt; GSM28745     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000     0.8548 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000     0.8548 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0000     0.9804 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28740     1  0.0000     0.8548 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     5  0.2605     0.7314 0.148 0.000 0.000 0.000 0.852
#&gt; GSM28726     5  0.2583     0.6753 0.000 0.004 0.000 0.132 0.864
#&gt; GSM28743     1  0.0000     0.8548 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.0000     0.7837 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11262     1  0.0000     0.8548 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28724     5  0.6053     0.5269 0.292 0.000 0.004 0.136 0.568
#&gt; GSM28725     3  0.0000     0.9804 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000     0.9804 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000     0.9804 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     4  0.0162     0.7854 0.004 0.000 0.000 0.996 0.000
#&gt; GSM28734     4  0.2300     0.7567 0.024 0.000 0.072 0.904 0.000
#&gt; GSM28747     5  0.4957     0.3565 0.444 0.000 0.000 0.028 0.528
#&gt; GSM11257     4  0.1671     0.7395 0.000 0.000 0.000 0.924 0.076
#&gt; GSM11252     1  0.6775     0.3056 0.528 0.000 0.104 0.316 0.052
#&gt; GSM11264     3  0.0000     0.9804 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.0000     0.9804 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11258     4  0.3242     0.6376 0.216 0.000 0.000 0.784 0.000
#&gt; GSM28728     5  0.2464     0.7303 0.096 0.000 0.000 0.016 0.888
#&gt; GSM28746     4  0.6614    -0.0518 0.416 0.000 0.012 0.424 0.148
#&gt; GSM28738     5  0.4101     0.4159 0.000 0.000 0.000 0.372 0.628
#&gt; GSM28741     2  0.0290     0.9913 0.000 0.992 0.000 0.000 0.008
#&gt; GSM28729     5  0.2338     0.6870 0.004 0.000 0.000 0.112 0.884
#&gt; GSM28742     5  0.2280     0.6825 0.000 0.000 0.000 0.120 0.880
#&gt; GSM11250     2  0.0000     0.9991 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11245     1  0.6938     0.2285 0.492 0.000 0.148 0.324 0.036
#&gt; GSM11246     1  0.0162     0.8526 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11261     3  0.0290     0.9747 0.000 0.000 0.992 0.008 0.000
#&gt; GSM11248     3  0.0324     0.9758 0.000 0.000 0.992 0.004 0.004
#&gt; GSM28732     5  0.2848     0.7286 0.156 0.000 0.000 0.004 0.840
#&gt; GSM11255     1  0.4195     0.7202 0.812 0.000 0.056 0.096 0.036
#&gt; GSM28731     5  0.4288     0.4847 0.384 0.000 0.000 0.004 0.612
#&gt; GSM28727     5  0.3305     0.7065 0.224 0.000 0.000 0.000 0.776
#&gt; GSM11251     5  0.3586     0.6777 0.264 0.000 0.000 0.000 0.736
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.4514      0.456 0.008 0.000 0.000 0.044 0.660 0.288
#&gt; GSM28736     5  0.5695      0.381 0.000 0.076 0.000 0.044 0.564 0.316
#&gt; GSM28737     1  0.0405      0.889 0.988 0.000 0.000 0.000 0.008 0.004
#&gt; GSM11249     3  0.3977      0.725 0.000 0.000 0.760 0.096 0.000 0.144
#&gt; GSM28745     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000      0.893 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.893 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0146      0.939 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28740     1  0.0000      0.893 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     5  0.2775      0.555 0.104 0.000 0.000 0.000 0.856 0.040
#&gt; GSM28726     5  0.4401      0.356 0.000 0.000 0.000 0.024 0.512 0.464
#&gt; GSM28743     1  0.0458      0.888 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM11256     4  0.0260      0.860 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM11262     1  0.0458      0.888 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM28724     5  0.5551      0.414 0.160 0.000 0.000 0.044 0.648 0.148
#&gt; GSM28725     3  0.0000      0.940 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.940 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.940 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0146      0.861 0.000 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28734     4  0.0622      0.849 0.000 0.000 0.008 0.980 0.000 0.012
#&gt; GSM28747     5  0.5595      0.322 0.268 0.000 0.000 0.000 0.540 0.192
#&gt; GSM11257     4  0.3641      0.654 0.000 0.000 0.000 0.748 0.028 0.224
#&gt; GSM11252     6  0.7479      0.445 0.248 0.000 0.056 0.196 0.052 0.448
#&gt; GSM11264     3  0.0000      0.940 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0146      0.939 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11258     4  0.2300      0.730 0.144 0.000 0.000 0.856 0.000 0.000
#&gt; GSM28728     5  0.3431      0.547 0.056 0.000 0.008 0.016 0.840 0.080
#&gt; GSM28746     6  0.7766      0.286 0.244 0.000 0.008 0.272 0.156 0.320
#&gt; GSM28738     6  0.5763     -0.343 0.000 0.000 0.000 0.180 0.356 0.464
#&gt; GSM28741     2  0.0914      0.967 0.000 0.968 0.000 0.000 0.016 0.016
#&gt; GSM28729     5  0.4830      0.279 0.004 0.000 0.000 0.044 0.496 0.456
#&gt; GSM28742     5  0.4331      0.358 0.000 0.000 0.000 0.020 0.516 0.464
#&gt; GSM11250     2  0.0000      0.997 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11245     6  0.7536      0.449 0.240 0.000 0.068 0.196 0.048 0.448
#&gt; GSM11246     1  0.0547      0.880 0.980 0.000 0.000 0.000 0.020 0.000
#&gt; GSM11261     3  0.1180      0.917 0.000 0.004 0.960 0.024 0.008 0.004
#&gt; GSM11248     3  0.2983      0.812 0.000 0.000 0.832 0.032 0.000 0.136
#&gt; GSM28732     5  0.3983      0.466 0.056 0.000 0.000 0.000 0.736 0.208
#&gt; GSM11255     1  0.5802     -0.193 0.472 0.000 0.012 0.060 0.028 0.428
#&gt; GSM28731     5  0.5991      0.282 0.260 0.000 0.000 0.004 0.480 0.256
#&gt; GSM28727     5  0.3772      0.536 0.160 0.000 0.000 0.000 0.772 0.068
#&gt; GSM11251     5  0.4135      0.468 0.300 0.000 0.000 0.000 0.668 0.032
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> CV:skmeans 48     0.392 2
#> CV:skmeans 45     0.447 3
#> CV:skmeans 40     0.406 4
#> CV:skmeans 43     0.463 5
#> CV:skmeans 35     0.435 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3511 0.650   0.650
#> 3 3 1.000           0.975       0.990         0.6008 0.798   0.688
#> 4 4 0.876           0.965       0.974         0.1616 0.912   0.803
#> 5 5 0.764           0.734       0.879         0.1246 0.910   0.750
#> 6 6 0.805           0.712       0.842         0.0679 0.913   0.706
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.0000      1.000 1.000 0.000
#&gt; GSM28736     1  0.0938      0.988 0.988 0.012
#&gt; GSM28737     1  0.0000      1.000 1.000 0.000
#&gt; GSM11249     1  0.0000      1.000 1.000 0.000
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000
#&gt; GSM11265     1  0.0000      1.000 1.000 0.000
#&gt; GSM28739     1  0.0000      1.000 1.000 0.000
#&gt; GSM11243     1  0.0000      1.000 1.000 0.000
#&gt; GSM28740     1  0.0000      1.000 1.000 0.000
#&gt; GSM11259     1  0.0000      1.000 1.000 0.000
#&gt; GSM28726     1  0.0000      1.000 1.000 0.000
#&gt; GSM28743     1  0.0000      1.000 1.000 0.000
#&gt; GSM11256     1  0.0000      1.000 1.000 0.000
#&gt; GSM11262     1  0.0000      1.000 1.000 0.000
#&gt; GSM28724     1  0.0000      1.000 1.000 0.000
#&gt; GSM28725     1  0.0000      1.000 1.000 0.000
#&gt; GSM11263     1  0.0000      1.000 1.000 0.000
#&gt; GSM11267     1  0.0000      1.000 1.000 0.000
#&gt; GSM28744     1  0.0000      1.000 1.000 0.000
#&gt; GSM28734     1  0.0000      1.000 1.000 0.000
#&gt; GSM28747     1  0.0000      1.000 1.000 0.000
#&gt; GSM11257     1  0.0000      1.000 1.000 0.000
#&gt; GSM11252     1  0.0000      1.000 1.000 0.000
#&gt; GSM11264     1  0.0000      1.000 1.000 0.000
#&gt; GSM11247     1  0.0000      1.000 1.000 0.000
#&gt; GSM11258     1  0.0000      1.000 1.000 0.000
#&gt; GSM28728     1  0.0000      1.000 1.000 0.000
#&gt; GSM28746     1  0.0000      1.000 1.000 0.000
#&gt; GSM28738     1  0.0000      1.000 1.000 0.000
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000
#&gt; GSM28729     1  0.0000      1.000 1.000 0.000
#&gt; GSM28742     1  0.0000      1.000 1.000 0.000
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000
#&gt; GSM11245     1  0.0000      1.000 1.000 0.000
#&gt; GSM11246     1  0.0000      1.000 1.000 0.000
#&gt; GSM11261     1  0.0000      1.000 1.000 0.000
#&gt; GSM11248     1  0.0000      1.000 1.000 0.000
#&gt; GSM28732     1  0.0000      1.000 1.000 0.000
#&gt; GSM11255     1  0.0000      1.000 1.000 0.000
#&gt; GSM28731     1  0.0000      1.000 1.000 0.000
#&gt; GSM28727     1  0.0000      1.000 1.000 0.000
#&gt; GSM11251     1  0.0000      1.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28736     1  0.0592      0.972 0.988 0.012 0.000
#&gt; GSM28737     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11249     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11265     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28740     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11259     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28726     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28743     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11256     1  0.3412      0.854 0.876 0.000 0.124
#&gt; GSM11262     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28724     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28744     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28734     1  0.5988      0.434 0.632 0.000 0.368
#&gt; GSM28747     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11257     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11252     1  0.0237      0.980 0.996 0.000 0.004
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11258     1  0.0237      0.980 0.996 0.000 0.004
#&gt; GSM28728     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28746     1  0.0237      0.980 0.996 0.000 0.004
#&gt; GSM28738     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28729     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28742     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11245     1  0.0237      0.980 0.996 0.000 0.004
#&gt; GSM11246     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11261     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11248     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28732     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11255     1  0.0237      0.980 0.996 0.000 0.004
#&gt; GSM28731     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.982 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.982 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM28736     1  0.0469      0.948 0.988 0.012 0.000 0.000
#&gt; GSM28737     1  0.2408      0.931 0.896 0.000 0.000 0.104
#&gt; GSM11249     3  0.1557      0.941 0.000 0.000 0.944 0.056
#&gt; GSM28745     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.2469      0.929 0.892 0.000 0.000 0.108
#&gt; GSM28739     1  0.2469      0.929 0.892 0.000 0.000 0.108
#&gt; GSM11243     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM28740     1  0.2469      0.929 0.892 0.000 0.000 0.108
#&gt; GSM11259     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM28726     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM28743     1  0.2469      0.929 0.892 0.000 0.000 0.108
#&gt; GSM11256     4  0.0336      0.989 0.008 0.000 0.000 0.992
#&gt; GSM11262     1  0.2469      0.929 0.892 0.000 0.000 0.108
#&gt; GSM28724     1  0.1940      0.940 0.924 0.000 0.000 0.076
#&gt; GSM28725     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.0336      0.989 0.008 0.000 0.000 0.992
#&gt; GSM28734     4  0.0592      0.979 0.000 0.000 0.016 0.984
#&gt; GSM28747     1  0.0469      0.954 0.988 0.000 0.000 0.012
#&gt; GSM11257     1  0.0592      0.952 0.984 0.000 0.000 0.016
#&gt; GSM11252     1  0.1940      0.940 0.924 0.000 0.000 0.076
#&gt; GSM11264     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM11258     4  0.0000      0.985 0.000 0.000 0.000 1.000
#&gt; GSM28728     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM28746     1  0.0336      0.954 0.992 0.000 0.000 0.008
#&gt; GSM28738     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM28741     2  0.0817      0.968 0.024 0.976 0.000 0.000
#&gt; GSM28729     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM28742     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM11250     2  0.0000      0.997 0.000 1.000 0.000 0.000
#&gt; GSM11245     1  0.2469      0.929 0.892 0.000 0.000 0.108
#&gt; GSM11246     1  0.0336      0.954 0.992 0.000 0.000 0.008
#&gt; GSM11261     1  0.2593      0.929 0.892 0.000 0.004 0.104
#&gt; GSM11248     3  0.0000      0.992 0.000 0.000 1.000 0.000
#&gt; GSM28732     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM11255     1  0.2345      0.932 0.900 0.000 0.000 0.100
#&gt; GSM28731     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.955 1.000 0.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.955 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3    p4    p5
#&gt; GSM28735     1  0.4273      0.137 0.552 0.00 0.000 0.000 0.448
#&gt; GSM28736     5  0.3684      0.678 0.280 0.00 0.000 0.000 0.720
#&gt; GSM28737     1  0.0566      0.654 0.984 0.00 0.000 0.004 0.012
#&gt; GSM11249     3  0.3210      0.873 0.008 0.00 0.860 0.040 0.092
#&gt; GSM28745     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11265     1  0.0290      0.650 0.992 0.00 0.000 0.008 0.000
#&gt; GSM28739     1  0.0290      0.650 0.992 0.00 0.000 0.008 0.000
#&gt; GSM11243     3  0.0000      0.966 0.000 0.00 1.000 0.000 0.000
#&gt; GSM28740     1  0.0290      0.650 0.992 0.00 0.000 0.008 0.000
#&gt; GSM11259     1  0.4138      0.337 0.616 0.00 0.000 0.000 0.384
#&gt; GSM28726     5  0.4030      0.591 0.352 0.00 0.000 0.000 0.648
#&gt; GSM28743     1  0.0290      0.650 0.992 0.00 0.000 0.008 0.000
#&gt; GSM11256     4  0.0000      0.994 0.000 0.00 0.000 1.000 0.000
#&gt; GSM11262     1  0.0290      0.650 0.992 0.00 0.000 0.008 0.000
#&gt; GSM28724     1  0.3586      0.499 0.736 0.00 0.000 0.000 0.264
#&gt; GSM28725     3  0.0000      0.966 0.000 0.00 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.966 0.000 0.00 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.966 0.000 0.00 1.000 0.000 0.000
#&gt; GSM28744     4  0.0000      0.994 0.000 0.00 0.000 1.000 0.000
#&gt; GSM28734     4  0.0000      0.994 0.000 0.00 0.000 1.000 0.000
#&gt; GSM28747     1  0.4150      0.332 0.612 0.00 0.000 0.000 0.388
#&gt; GSM11257     5  0.3612      0.527 0.268 0.00 0.000 0.000 0.732
#&gt; GSM11252     1  0.3796      0.448 0.700 0.00 0.000 0.000 0.300
#&gt; GSM11264     3  0.0000      0.966 0.000 0.00 1.000 0.000 0.000
#&gt; GSM11247     3  0.0000      0.966 0.000 0.00 1.000 0.000 0.000
#&gt; GSM11258     4  0.0510      0.981 0.016 0.00 0.000 0.984 0.000
#&gt; GSM28728     1  0.3534      0.552 0.744 0.00 0.000 0.000 0.256
#&gt; GSM28746     1  0.1270      0.655 0.948 0.00 0.000 0.000 0.052
#&gt; GSM28738     5  0.3561      0.577 0.260 0.00 0.000 0.000 0.740
#&gt; GSM28741     2  0.2280      0.860 0.000 0.88 0.000 0.000 0.120
#&gt; GSM28729     1  0.4101      0.363 0.628 0.00 0.000 0.000 0.372
#&gt; GSM28742     5  0.3661      0.670 0.276 0.00 0.000 0.000 0.724
#&gt; GSM11250     2  0.0000      0.987 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11245     1  0.2561      0.564 0.856 0.00 0.000 0.000 0.144
#&gt; GSM11246     1  0.1544      0.655 0.932 0.00 0.000 0.000 0.068
#&gt; GSM11261     1  0.2171      0.631 0.912 0.00 0.064 0.000 0.024
#&gt; GSM11248     3  0.2193      0.900 0.008 0.00 0.900 0.000 0.092
#&gt; GSM28732     1  0.3707      0.517 0.716 0.00 0.000 0.000 0.284
#&gt; GSM11255     1  0.1478      0.587 0.936 0.00 0.000 0.000 0.064
#&gt; GSM28731     1  0.3534      0.552 0.744 0.00 0.000 0.000 0.256
#&gt; GSM28727     1  0.4161      0.321 0.608 0.00 0.000 0.000 0.392
#&gt; GSM11251     1  0.4138      0.337 0.616 0.00 0.000 0.000 0.384
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     1  0.3318     0.2807 0.796 0.000 0.000 0.000 0.172 0.032
#&gt; GSM28736     5  0.3747     0.9030 0.396 0.000 0.000 0.000 0.604 0.000
#&gt; GSM28737     1  0.3706     0.6320 0.620 0.000 0.000 0.000 0.380 0.000
#&gt; GSM11249     6  0.3647     0.3261 0.000 0.000 0.360 0.000 0.000 0.640
#&gt; GSM28745     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3727     0.6289 0.612 0.000 0.000 0.000 0.388 0.000
#&gt; GSM28739     1  0.3727     0.6289 0.612 0.000 0.000 0.000 0.388 0.000
#&gt; GSM11243     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28740     1  0.3727     0.6289 0.612 0.000 0.000 0.000 0.388 0.000
#&gt; GSM11259     1  0.0000     0.6078 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28726     5  0.3747     0.9030 0.396 0.000 0.000 0.000 0.604 0.000
#&gt; GSM28743     1  0.3727     0.6289 0.612 0.000 0.000 0.000 0.388 0.000
#&gt; GSM11256     4  0.0000     0.9900 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11262     1  0.3727     0.6289 0.612 0.000 0.000 0.000 0.388 0.000
#&gt; GSM28724     1  0.2402     0.6560 0.856 0.000 0.000 0.000 0.140 0.004
#&gt; GSM28725     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0000     0.9900 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28734     4  0.0000     0.9900 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28747     1  0.0146     0.6060 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM11257     6  0.1757     0.2770 0.076 0.000 0.000 0.000 0.008 0.916
#&gt; GSM11252     6  0.3620     0.4913 0.352 0.000 0.000 0.000 0.000 0.648
#&gt; GSM11264     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11258     4  0.0632     0.9699 0.000 0.000 0.000 0.976 0.024 0.000
#&gt; GSM28728     1  0.0000     0.6078 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28746     1  0.5137     0.5677 0.596 0.000 0.000 0.000 0.284 0.120
#&gt; GSM28738     1  0.5818    -0.6398 0.456 0.000 0.000 0.000 0.192 0.352
#&gt; GSM28741     2  0.2046     0.8879 0.060 0.908 0.000 0.000 0.032 0.000
#&gt; GSM28729     1  0.1088     0.5651 0.960 0.000 0.000 0.000 0.024 0.016
#&gt; GSM28742     5  0.5759     0.7792 0.392 0.000 0.000 0.000 0.436 0.172
#&gt; GSM11250     2  0.0000     0.9897 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11245     6  0.3620     0.4913 0.352 0.000 0.000 0.000 0.000 0.648
#&gt; GSM11246     1  0.3531     0.6434 0.672 0.000 0.000 0.000 0.328 0.000
#&gt; GSM11261     1  0.5363     0.5379 0.608 0.000 0.196 0.000 0.192 0.004
#&gt; GSM11248     6  0.3620     0.3384 0.000 0.000 0.352 0.000 0.000 0.648
#&gt; GSM28732     1  0.3789     0.0867 0.584 0.000 0.000 0.000 0.000 0.416
#&gt; GSM11255     6  0.3847     0.2465 0.456 0.000 0.000 0.000 0.000 0.544
#&gt; GSM28731     1  0.0000     0.6078 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28727     1  0.0363     0.6013 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM11251     1  0.0000     0.6078 1.000 0.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> CV:pam 50     0.394 2
#> CV:pam 49     0.368 3
#> CV:pam 50     0.512 4
#> CV:pam 42     0.457 5
#> CV:pam 41     0.448 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.499           0.882       0.913         0.4503 0.519   0.519
#> 3 3 0.811           0.856       0.935         0.3889 0.760   0.574
#> 4 4 0.773           0.807       0.898         0.0629 0.909   0.776
#> 5 5 0.745           0.799       0.885         0.1234 0.845   0.591
#> 6 6 0.762           0.815       0.860         0.0766 0.894   0.605
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.2778      0.935 0.952 0.048
#&gt; GSM28736     1  0.5178      0.886 0.884 0.116
#&gt; GSM28737     1  0.0000      0.951 1.000 0.000
#&gt; GSM11249     2  0.8909      0.770 0.308 0.692
#&gt; GSM28745     2  0.1843      0.834 0.028 0.972
#&gt; GSM11244     2  0.1843      0.834 0.028 0.972
#&gt; GSM28748     2  0.1843      0.834 0.028 0.972
#&gt; GSM11266     2  0.1843      0.834 0.028 0.972
#&gt; GSM28730     2  0.1843      0.834 0.028 0.972
#&gt; GSM11253     2  0.1843      0.834 0.028 0.972
#&gt; GSM11254     2  0.1843      0.834 0.028 0.972
#&gt; GSM11260     2  0.1843      0.834 0.028 0.972
#&gt; GSM28733     2  0.1843      0.834 0.028 0.972
#&gt; GSM11265     1  0.0000      0.951 1.000 0.000
#&gt; GSM28739     1  0.0000      0.951 1.000 0.000
#&gt; GSM11243     2  0.8909      0.770 0.308 0.692
#&gt; GSM28740     1  0.0000      0.951 1.000 0.000
#&gt; GSM11259     1  0.0000      0.951 1.000 0.000
#&gt; GSM28726     1  0.4939      0.894 0.892 0.108
#&gt; GSM28743     1  0.0000      0.951 1.000 0.000
#&gt; GSM11256     1  0.6048      0.879 0.852 0.148
#&gt; GSM11262     1  0.1633      0.943 0.976 0.024
#&gt; GSM28724     1  0.0000      0.951 1.000 0.000
#&gt; GSM28725     2  0.8909      0.770 0.308 0.692
#&gt; GSM11263     2  0.8909      0.770 0.308 0.692
#&gt; GSM11267     2  0.8909      0.770 0.308 0.692
#&gt; GSM28744     1  0.6048      0.879 0.852 0.148
#&gt; GSM28734     1  0.6048      0.879 0.852 0.148
#&gt; GSM28747     1  0.0000      0.951 1.000 0.000
#&gt; GSM11257     1  0.5294      0.887 0.880 0.120
#&gt; GSM11252     1  0.0376      0.948 0.996 0.004
#&gt; GSM11264     2  0.8909      0.770 0.308 0.692
#&gt; GSM11247     2  0.8909      0.770 0.308 0.692
#&gt; GSM11258     1  0.6048      0.879 0.852 0.148
#&gt; GSM28728     1  0.0000      0.951 1.000 0.000
#&gt; GSM28746     1  0.0376      0.948 0.996 0.004
#&gt; GSM28738     1  0.2778      0.935 0.952 0.048
#&gt; GSM28741     1  0.5629      0.877 0.868 0.132
#&gt; GSM28729     1  0.2778      0.935 0.952 0.048
#&gt; GSM28742     1  0.2778      0.935 0.952 0.048
#&gt; GSM11250     2  0.1843      0.834 0.028 0.972
#&gt; GSM11245     1  0.0376      0.948 0.996 0.004
#&gt; GSM11246     1  0.0000      0.951 1.000 0.000
#&gt; GSM11261     2  0.8909      0.770 0.308 0.692
#&gt; GSM11248     2  0.8909      0.770 0.308 0.692
#&gt; GSM28732     1  0.0000      0.951 1.000 0.000
#&gt; GSM11255     1  0.0000      0.951 1.000 0.000
#&gt; GSM28731     1  0.0000      0.951 1.000 0.000
#&gt; GSM28727     1  0.0000      0.951 1.000 0.000
#&gt; GSM11251     1  0.0000      0.951 1.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1   0.280      0.894 0.908 0.000 0.092
#&gt; GSM28736     1   0.611      0.391 0.604 0.000 0.396
#&gt; GSM28737     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM11249     3   0.129      0.936 0.032 0.000 0.968
#&gt; GSM28745     2   0.000      0.884 0.000 1.000 0.000
#&gt; GSM11244     2   0.000      0.884 0.000 1.000 0.000
#&gt; GSM28748     2   0.630      0.133 0.000 0.524 0.476
#&gt; GSM11266     2   0.000      0.884 0.000 1.000 0.000
#&gt; GSM28730     2   0.000      0.884 0.000 1.000 0.000
#&gt; GSM11253     2   0.000      0.884 0.000 1.000 0.000
#&gt; GSM11254     2   0.000      0.884 0.000 1.000 0.000
#&gt; GSM11260     2   0.000      0.884 0.000 1.000 0.000
#&gt; GSM28733     2   0.000      0.884 0.000 1.000 0.000
#&gt; GSM11265     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM28739     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM11243     3   0.000      0.929 0.000 0.000 1.000
#&gt; GSM28740     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM11259     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM28726     1   0.382      0.847 0.852 0.000 0.148
#&gt; GSM28743     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM11256     3   0.129      0.936 0.032 0.000 0.968
#&gt; GSM11262     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM28724     1   0.280      0.894 0.908 0.000 0.092
#&gt; GSM28725     3   0.000      0.929 0.000 0.000 1.000
#&gt; GSM11263     3   0.000      0.929 0.000 0.000 1.000
#&gt; GSM11267     3   0.000      0.929 0.000 0.000 1.000
#&gt; GSM28744     3   0.129      0.936 0.032 0.000 0.968
#&gt; GSM28734     3   0.129      0.936 0.032 0.000 0.968
#&gt; GSM28747     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM11257     3   0.455      0.732 0.200 0.000 0.800
#&gt; GSM11252     1   0.129      0.923 0.968 0.000 0.032
#&gt; GSM11264     3   0.000      0.929 0.000 0.000 1.000
#&gt; GSM11247     3   0.000      0.929 0.000 0.000 1.000
#&gt; GSM11258     3   0.153      0.929 0.040 0.000 0.960
#&gt; GSM28728     1   0.271      0.896 0.912 0.000 0.088
#&gt; GSM28746     1   0.186      0.913 0.948 0.000 0.052
#&gt; GSM28738     1   0.581      0.521 0.664 0.000 0.336
#&gt; GSM28741     3   0.810      0.568 0.200 0.152 0.648
#&gt; GSM28729     1   0.271      0.896 0.912 0.000 0.088
#&gt; GSM28742     1   0.280      0.894 0.908 0.000 0.092
#&gt; GSM11250     2   0.630      0.133 0.000 0.524 0.476
#&gt; GSM11245     1   0.153      0.920 0.960 0.000 0.040
#&gt; GSM11246     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM11261     3   0.129      0.936 0.032 0.000 0.968
#&gt; GSM11248     3   0.129      0.936 0.032 0.000 0.968
#&gt; GSM28732     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM11255     1   0.141      0.922 0.964 0.000 0.036
#&gt; GSM28731     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM28727     1   0.000      0.933 1.000 0.000 0.000
#&gt; GSM11251     1   0.000      0.933 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.2081     0.7624 0.916 0.000 0.000 0.084
#&gt; GSM28736     1  0.2216     0.7551 0.908 0.000 0.000 0.092
#&gt; GSM28737     1  0.3266     0.8174 0.832 0.000 0.000 0.168
#&gt; GSM11249     3  0.1452     0.8751 0.036 0.000 0.956 0.008
#&gt; GSM28745     2  0.0000     0.9742 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9742 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0779     0.9541 0.016 0.980 0.000 0.004
#&gt; GSM11266     2  0.0000     0.9742 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9742 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9742 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9742 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9742 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9742 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.3444     0.8096 0.816 0.000 0.000 0.184
#&gt; GSM28739     1  0.3444     0.8096 0.816 0.000 0.000 0.184
#&gt; GSM11243     3  0.0000     0.8973 0.000 0.000 1.000 0.000
#&gt; GSM28740     1  0.3444     0.8096 0.816 0.000 0.000 0.184
#&gt; GSM11259     1  0.3266     0.8174 0.832 0.000 0.000 0.168
#&gt; GSM28726     1  0.2216     0.7551 0.908 0.000 0.000 0.092
#&gt; GSM28743     1  0.3444     0.8096 0.816 0.000 0.000 0.184
#&gt; GSM11256     4  0.3444     0.8831 0.184 0.000 0.000 0.816
#&gt; GSM11262     1  0.3356     0.8138 0.824 0.000 0.000 0.176
#&gt; GSM28724     1  0.1940     0.7672 0.924 0.000 0.000 0.076
#&gt; GSM28725     3  0.0000     0.8973 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000     0.8973 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000     0.8973 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.3444     0.8831 0.184 0.000 0.000 0.816
#&gt; GSM28734     4  0.3444     0.8831 0.184 0.000 0.000 0.816
#&gt; GSM28747     1  0.3266     0.8174 0.832 0.000 0.000 0.168
#&gt; GSM11257     1  0.4072     0.4862 0.748 0.000 0.000 0.252
#&gt; GSM11252     1  0.0469     0.7959 0.988 0.000 0.000 0.012
#&gt; GSM11264     3  0.0000     0.8973 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0707     0.8901 0.020 0.000 0.980 0.000
#&gt; GSM11258     4  0.4830     0.6156 0.392 0.000 0.000 0.608
#&gt; GSM28728     1  0.2081     0.7624 0.916 0.000 0.000 0.084
#&gt; GSM28746     1  0.0336     0.7974 0.992 0.000 0.000 0.008
#&gt; GSM28738     1  0.2081     0.7624 0.916 0.000 0.000 0.084
#&gt; GSM28741     1  0.6532     0.0533 0.572 0.336 0.000 0.092
#&gt; GSM28729     1  0.2081     0.7624 0.916 0.000 0.000 0.084
#&gt; GSM28742     1  0.2081     0.7624 0.916 0.000 0.000 0.084
#&gt; GSM11250     2  0.3447     0.7627 0.128 0.852 0.000 0.020
#&gt; GSM11245     1  0.0592     0.7945 0.984 0.000 0.000 0.016
#&gt; GSM11246     1  0.3444     0.8096 0.816 0.000 0.000 0.184
#&gt; GSM11261     3  0.5721     0.0995 0.412 0.008 0.564 0.016
#&gt; GSM11248     3  0.1978     0.8430 0.068 0.000 0.928 0.004
#&gt; GSM28732     1  0.3266     0.8174 0.832 0.000 0.000 0.168
#&gt; GSM11255     1  0.0188     0.7981 0.996 0.000 0.000 0.004
#&gt; GSM28731     1  0.3266     0.8174 0.832 0.000 0.000 0.168
#&gt; GSM28727     1  0.3266     0.8174 0.832 0.000 0.000 0.168
#&gt; GSM11251     1  0.3266     0.8174 0.832 0.000 0.000 0.168
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.3876      0.798 0.316 0.000 0.000 0.000 0.684
#&gt; GSM28736     5  0.2690      0.837 0.156 0.000 0.000 0.000 0.844
#&gt; GSM28737     1  0.0290      0.761 0.992 0.000 0.000 0.000 0.008
#&gt; GSM11249     3  0.0703      0.970 0.000 0.000 0.976 0.000 0.024
#&gt; GSM28745     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.979 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.2690      0.696 0.844 0.000 0.000 0.000 0.156
#&gt; GSM28739     1  0.2690      0.696 0.844 0.000 0.000 0.000 0.156
#&gt; GSM11243     3  0.0000      0.990 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28740     1  0.2773      0.693 0.836 0.000 0.000 0.000 0.164
#&gt; GSM11259     1  0.0162      0.762 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28726     5  0.2690      0.837 0.156 0.000 0.000 0.000 0.844
#&gt; GSM28743     1  0.2773      0.693 0.836 0.000 0.000 0.000 0.164
#&gt; GSM11256     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11262     1  0.1478      0.726 0.936 0.000 0.000 0.000 0.064
#&gt; GSM28724     1  0.4292      0.460 0.704 0.000 0.024 0.000 0.272
#&gt; GSM28725     3  0.0000      0.990 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.990 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.990 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28734     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28747     1  0.0162      0.762 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11257     5  0.3388      0.841 0.200 0.000 0.000 0.008 0.792
#&gt; GSM11252     1  0.3534      0.520 0.744 0.000 0.000 0.000 0.256
#&gt; GSM11264     3  0.0000      0.990 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.0000      0.990 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11258     1  0.6263      0.160 0.532 0.000 0.000 0.192 0.276
#&gt; GSM28728     1  0.4734      0.106 0.604 0.000 0.024 0.000 0.372
#&gt; GSM28746     1  0.3480      0.533 0.752 0.000 0.000 0.000 0.248
#&gt; GSM28738     5  0.3876      0.799 0.316 0.000 0.000 0.000 0.684
#&gt; GSM28741     5  0.2690      0.837 0.156 0.000 0.000 0.000 0.844
#&gt; GSM28729     5  0.4235      0.583 0.424 0.000 0.000 0.000 0.576
#&gt; GSM28742     5  0.3949      0.779 0.332 0.000 0.000 0.000 0.668
#&gt; GSM11250     2  0.2471      0.801 0.000 0.864 0.000 0.000 0.136
#&gt; GSM11245     1  0.3534      0.520 0.744 0.000 0.000 0.000 0.256
#&gt; GSM11246     1  0.2690      0.696 0.844 0.000 0.000 0.000 0.156
#&gt; GSM11261     5  0.4617      0.786 0.148 0.000 0.108 0.000 0.744
#&gt; GSM11248     3  0.0703      0.970 0.000 0.000 0.976 0.000 0.024
#&gt; GSM28732     1  0.0162      0.762 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11255     1  0.3480      0.531 0.752 0.000 0.000 0.000 0.248
#&gt; GSM28731     1  0.0000      0.762 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28727     1  0.0162      0.762 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11251     1  0.0000      0.762 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.2743      0.669 0.164 0.000 0.000 0.000 0.828 0.008
#&gt; GSM28736     5  0.0000      0.641 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28737     1  0.0717      0.893 0.976 0.000 0.000 0.000 0.016 0.008
#&gt; GSM11249     3  0.2527      0.859 0.000 0.000 0.832 0.000 0.000 0.168
#&gt; GSM28745     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0622      0.953 0.000 0.980 0.000 0.000 0.008 0.012
#&gt; GSM11266     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.967 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.2491      0.850 0.836 0.000 0.000 0.000 0.000 0.164
#&gt; GSM28739     1  0.2562      0.844 0.828 0.000 0.000 0.000 0.000 0.172
#&gt; GSM11243     3  0.0000      0.957 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28740     1  0.2593      0.853 0.844 0.000 0.000 0.000 0.008 0.148
#&gt; GSM11259     1  0.0806      0.891 0.972 0.000 0.000 0.000 0.020 0.008
#&gt; GSM28726     5  0.0000      0.641 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28743     1  0.2706      0.845 0.832 0.000 0.000 0.000 0.008 0.160
#&gt; GSM11256     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11262     1  0.1918      0.839 0.904 0.000 0.000 0.000 0.088 0.008
#&gt; GSM28724     5  0.5561      0.335 0.308 0.000 0.000 0.000 0.528 0.164
#&gt; GSM28725     3  0.0000      0.957 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.957 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.957 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28734     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28747     1  0.0692      0.891 0.976 0.000 0.000 0.000 0.020 0.004
#&gt; GSM11257     5  0.3714      0.628 0.052 0.000 0.000 0.024 0.808 0.116
#&gt; GSM11252     6  0.4915      0.889 0.188 0.000 0.000 0.000 0.156 0.656
#&gt; GSM11264     3  0.0000      0.957 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0000      0.957 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11258     5  0.7074      0.128 0.108 0.000 0.000 0.188 0.440 0.264
#&gt; GSM28728     5  0.5670      0.359 0.296 0.000 0.000 0.000 0.516 0.188
#&gt; GSM28746     6  0.4915      0.889 0.188 0.000 0.000 0.000 0.156 0.656
#&gt; GSM28738     5  0.2743      0.670 0.164 0.000 0.000 0.000 0.828 0.008
#&gt; GSM28741     5  0.0713      0.631 0.000 0.000 0.000 0.000 0.972 0.028
#&gt; GSM28729     5  0.4585      0.545 0.284 0.000 0.000 0.000 0.648 0.068
#&gt; GSM28742     5  0.2664      0.658 0.184 0.000 0.000 0.000 0.816 0.000
#&gt; GSM11250     2  0.3543      0.683 0.000 0.768 0.000 0.000 0.200 0.032
#&gt; GSM11245     6  0.4910      0.888 0.192 0.000 0.000 0.000 0.152 0.656
#&gt; GSM11246     1  0.2416      0.850 0.844 0.000 0.000 0.000 0.000 0.156
#&gt; GSM11261     5  0.5485      0.337 0.020 0.000 0.076 0.000 0.516 0.388
#&gt; GSM11248     3  0.2454      0.865 0.000 0.000 0.840 0.000 0.000 0.160
#&gt; GSM28732     1  0.0937      0.873 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; GSM11255     6  0.5027      0.705 0.304 0.000 0.000 0.000 0.100 0.596
#&gt; GSM28731     1  0.0603      0.892 0.980 0.000 0.000 0.000 0.016 0.004
#&gt; GSM28727     1  0.0717      0.892 0.976 0.000 0.000 0.000 0.016 0.008
#&gt; GSM11251     1  0.0717      0.892 0.976 0.000 0.000 0.000 0.016 0.008
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:mclust 50     0.394 2
#> CV:mclust 47     0.437 3
#> CV:mclust 47     0.504 4
#> CV:mclust 47     0.512 5
#> CV:mclust 46     0.477 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.990       0.995         0.3933 0.607   0.607
#> 3 3 0.968           0.948       0.979         0.5141 0.718   0.566
#> 4 4 0.923           0.920       0.954         0.2296 0.838   0.616
#> 5 5 0.767           0.650       0.840         0.0614 0.904   0.664
#> 6 6 0.742           0.618       0.784         0.0592 0.892   0.554
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.0000      0.996 1.000 0.000
#&gt; GSM28736     2  0.2043      0.965 0.032 0.968
#&gt; GSM28737     1  0.0000      0.996 1.000 0.000
#&gt; GSM11249     1  0.0000      0.996 1.000 0.000
#&gt; GSM28745     2  0.0000      0.991 0.000 1.000
#&gt; GSM11244     2  0.0000      0.991 0.000 1.000
#&gt; GSM28748     2  0.0000      0.991 0.000 1.000
#&gt; GSM11266     2  0.0000      0.991 0.000 1.000
#&gt; GSM28730     2  0.0000      0.991 0.000 1.000
#&gt; GSM11253     2  0.0000      0.991 0.000 1.000
#&gt; GSM11254     2  0.0000      0.991 0.000 1.000
#&gt; GSM11260     2  0.0000      0.991 0.000 1.000
#&gt; GSM28733     2  0.0000      0.991 0.000 1.000
#&gt; GSM11265     1  0.0000      0.996 1.000 0.000
#&gt; GSM28739     1  0.0000      0.996 1.000 0.000
#&gt; GSM11243     1  0.0000      0.996 1.000 0.000
#&gt; GSM28740     1  0.0000      0.996 1.000 0.000
#&gt; GSM11259     1  0.0000      0.996 1.000 0.000
#&gt; GSM28726     2  0.4022      0.916 0.080 0.920
#&gt; GSM28743     1  0.0000      0.996 1.000 0.000
#&gt; GSM11256     1  0.0000      0.996 1.000 0.000
#&gt; GSM11262     1  0.0000      0.996 1.000 0.000
#&gt; GSM28724     1  0.0000      0.996 1.000 0.000
#&gt; GSM28725     1  0.0000      0.996 1.000 0.000
#&gt; GSM11263     1  0.0000      0.996 1.000 0.000
#&gt; GSM11267     1  0.0000      0.996 1.000 0.000
#&gt; GSM28744     1  0.0000      0.996 1.000 0.000
#&gt; GSM28734     1  0.0000      0.996 1.000 0.000
#&gt; GSM28747     1  0.0000      0.996 1.000 0.000
#&gt; GSM11257     1  0.0000      0.996 1.000 0.000
#&gt; GSM11252     1  0.0000      0.996 1.000 0.000
#&gt; GSM11264     1  0.0000      0.996 1.000 0.000
#&gt; GSM11247     1  0.0000      0.996 1.000 0.000
#&gt; GSM11258     1  0.0000      0.996 1.000 0.000
#&gt; GSM28728     1  0.0000      0.996 1.000 0.000
#&gt; GSM28746     1  0.0000      0.996 1.000 0.000
#&gt; GSM28738     1  0.4431      0.899 0.908 0.092
#&gt; GSM28741     2  0.0000      0.991 0.000 1.000
#&gt; GSM28729     1  0.0000      0.996 1.000 0.000
#&gt; GSM28742     1  0.1633      0.974 0.976 0.024
#&gt; GSM11250     2  0.0000      0.991 0.000 1.000
#&gt; GSM11245     1  0.0000      0.996 1.000 0.000
#&gt; GSM11246     1  0.0000      0.996 1.000 0.000
#&gt; GSM11261     1  0.0938      0.986 0.988 0.012
#&gt; GSM11248     1  0.0000      0.996 1.000 0.000
#&gt; GSM28732     1  0.0000      0.996 1.000 0.000
#&gt; GSM11255     1  0.0000      0.996 1.000 0.000
#&gt; GSM28731     1  0.0000      0.996 1.000 0.000
#&gt; GSM28727     1  0.0000      0.996 1.000 0.000
#&gt; GSM11251     1  0.0000      0.996 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28736     1  0.5882      0.478 0.652 0.348 0.000
#&gt; GSM28737     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11249     3  0.0000      0.971 0.000 0.000 1.000
#&gt; GSM28745     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11244     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28748     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11266     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28730     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11253     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11254     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11260     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28733     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11265     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11243     3  0.0000      0.971 0.000 0.000 1.000
#&gt; GSM28740     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11259     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28726     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28743     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11256     1  0.5859      0.476 0.656 0.000 0.344
#&gt; GSM11262     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28724     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28725     3  0.0000      0.971 0.000 0.000 1.000
#&gt; GSM11263     3  0.0000      0.971 0.000 0.000 1.000
#&gt; GSM11267     3  0.0000      0.971 0.000 0.000 1.000
#&gt; GSM28744     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28734     3  0.4399      0.732 0.188 0.000 0.812
#&gt; GSM28747     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11257     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11252     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM11264     3  0.0000      0.971 0.000 0.000 1.000
#&gt; GSM11247     3  0.0000      0.971 0.000 0.000 1.000
#&gt; GSM11258     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28728     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28746     1  0.0747      0.957 0.984 0.000 0.016
#&gt; GSM28738     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28741     2  0.0747      0.978 0.016 0.984 0.000
#&gt; GSM28729     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28742     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11250     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11245     1  0.3267      0.855 0.884 0.000 0.116
#&gt; GSM11246     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11261     3  0.0237      0.968 0.000 0.004 0.996
#&gt; GSM11248     3  0.0000      0.971 0.000 0.000 1.000
#&gt; GSM28732     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11255     1  0.0237      0.967 0.996 0.000 0.004
#&gt; GSM28731     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.969 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.969 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     4  0.4843      0.422 0.396 0.000 0.000 0.604
#&gt; GSM28736     4  0.2775      0.840 0.020 0.084 0.000 0.896
#&gt; GSM28737     1  0.0336      0.942 0.992 0.000 0.000 0.008
#&gt; GSM11249     3  0.0188      0.993 0.000 0.000 0.996 0.004
#&gt; GSM28745     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0188      0.997 0.000 0.996 0.000 0.004
#&gt; GSM11266     2  0.0188      0.997 0.000 0.996 0.000 0.004
#&gt; GSM28730     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.0336      0.942 0.992 0.000 0.000 0.008
#&gt; GSM28739     1  0.0336      0.942 0.992 0.000 0.000 0.008
#&gt; GSM11243     3  0.0524      0.989 0.004 0.000 0.988 0.008
#&gt; GSM28740     1  0.0336      0.942 0.992 0.000 0.000 0.008
#&gt; GSM11259     1  0.1389      0.926 0.952 0.000 0.000 0.048
#&gt; GSM28726     4  0.2300      0.874 0.064 0.016 0.000 0.920
#&gt; GSM28743     1  0.0592      0.939 0.984 0.000 0.000 0.016
#&gt; GSM11256     4  0.1004      0.876 0.024 0.000 0.004 0.972
#&gt; GSM11262     1  0.0592      0.939 0.984 0.000 0.000 0.016
#&gt; GSM28724     1  0.0817      0.941 0.976 0.000 0.000 0.024
#&gt; GSM28725     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.1302      0.876 0.044 0.000 0.000 0.956
#&gt; GSM28734     4  0.1767      0.858 0.012 0.000 0.044 0.944
#&gt; GSM28747     1  0.0336      0.942 0.992 0.000 0.000 0.008
#&gt; GSM11257     4  0.0817      0.876 0.024 0.000 0.000 0.976
#&gt; GSM11252     1  0.2300      0.910 0.924 0.000 0.028 0.048
#&gt; GSM11264     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0895      0.981 0.004 0.000 0.976 0.020
#&gt; GSM11258     1  0.4608      0.559 0.692 0.000 0.004 0.304
#&gt; GSM28728     1  0.1389      0.925 0.952 0.000 0.000 0.048
#&gt; GSM28746     1  0.3863      0.812 0.828 0.000 0.028 0.144
#&gt; GSM28738     4  0.3142      0.838 0.132 0.008 0.000 0.860
#&gt; GSM28741     2  0.0376      0.993 0.004 0.992 0.000 0.004
#&gt; GSM28729     4  0.3975      0.738 0.240 0.000 0.000 0.760
#&gt; GSM28742     4  0.0707      0.874 0.020 0.000 0.000 0.980
#&gt; GSM11250     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM11245     1  0.4678      0.690 0.744 0.000 0.232 0.024
#&gt; GSM11246     1  0.0188      0.942 0.996 0.000 0.000 0.004
#&gt; GSM11261     3  0.0188      0.993 0.004 0.000 0.996 0.000
#&gt; GSM11248     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM28732     1  0.0707      0.940 0.980 0.000 0.000 0.020
#&gt; GSM11255     1  0.0779      0.940 0.980 0.000 0.016 0.004
#&gt; GSM28731     1  0.0707      0.940 0.980 0.000 0.000 0.020
#&gt; GSM28727     1  0.0336      0.942 0.992 0.000 0.000 0.008
#&gt; GSM11251     1  0.0336      0.942 0.992 0.000 0.000 0.008
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.5223    -0.0985 0.444 0.000 0.000 0.044 0.512
#&gt; GSM28736     5  0.6021    -0.0706 0.004 0.124 0.000 0.312 0.560
#&gt; GSM28737     1  0.1478     0.6705 0.936 0.000 0.000 0.000 0.064
#&gt; GSM11249     3  0.0798     0.8937 0.008 0.000 0.976 0.016 0.000
#&gt; GSM28745     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0162     0.6618 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28739     1  0.0000     0.6632 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.1628     0.8732 0.000 0.000 0.936 0.008 0.056
#&gt; GSM28740     1  0.0290     0.6604 0.992 0.000 0.000 0.008 0.000
#&gt; GSM11259     5  0.4268    -0.0988 0.444 0.000 0.000 0.000 0.556
#&gt; GSM28726     5  0.3779     0.5893 0.124 0.004 0.000 0.056 0.816
#&gt; GSM28743     1  0.0609     0.6532 0.980 0.000 0.000 0.020 0.000
#&gt; GSM11256     4  0.1430     0.7365 0.004 0.000 0.000 0.944 0.052
#&gt; GSM11262     1  0.0963     0.6407 0.964 0.000 0.000 0.036 0.000
#&gt; GSM28724     1  0.4597     0.3702 0.564 0.000 0.000 0.012 0.424
#&gt; GSM28725     3  0.0162     0.9018 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11263     3  0.0000     0.9021 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0162     0.9017 0.000 0.000 0.996 0.004 0.000
#&gt; GSM28744     4  0.0865     0.7446 0.004 0.000 0.000 0.972 0.024
#&gt; GSM28734     4  0.1725     0.7379 0.044 0.000 0.000 0.936 0.020
#&gt; GSM28747     1  0.4015     0.5096 0.652 0.000 0.000 0.000 0.348
#&gt; GSM11257     4  0.4294     0.2701 0.000 0.000 0.000 0.532 0.468
#&gt; GSM11252     1  0.4920     0.5792 0.756 0.000 0.036 0.072 0.136
#&gt; GSM11264     3  0.0000     0.9021 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.2563     0.8188 0.000 0.000 0.872 0.008 0.120
#&gt; GSM11258     4  0.4161     0.4370 0.392 0.000 0.000 0.608 0.000
#&gt; GSM28728     5  0.4449     0.2129 0.352 0.000 0.004 0.008 0.636
#&gt; GSM28746     1  0.4874     0.4279 0.588 0.000 0.016 0.008 0.388
#&gt; GSM28738     5  0.1282     0.5439 0.000 0.000 0.004 0.044 0.952
#&gt; GSM28741     2  0.2338     0.8362 0.000 0.884 0.000 0.004 0.112
#&gt; GSM28729     5  0.2362     0.6103 0.076 0.000 0.000 0.024 0.900
#&gt; GSM28742     5  0.1041     0.5603 0.004 0.000 0.000 0.032 0.964
#&gt; GSM11250     2  0.0000     0.9848 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11245     3  0.6169     0.1616 0.444 0.000 0.464 0.064 0.028
#&gt; GSM11246     1  0.1478     0.6705 0.936 0.000 0.000 0.000 0.064
#&gt; GSM11261     3  0.0693     0.8975 0.000 0.000 0.980 0.008 0.012
#&gt; GSM11248     3  0.0404     0.8995 0.000 0.000 0.988 0.012 0.000
#&gt; GSM28732     1  0.4307     0.1672 0.500 0.000 0.000 0.000 0.500
#&gt; GSM11255     1  0.3670     0.6272 0.792 0.000 0.008 0.012 0.188
#&gt; GSM28731     1  0.4304     0.2136 0.516 0.000 0.000 0.000 0.484
#&gt; GSM28727     1  0.4210     0.4088 0.588 0.000 0.000 0.000 0.412
#&gt; GSM11251     1  0.3796     0.5422 0.700 0.000 0.000 0.000 0.300
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     6  0.4853     0.5113 0.156 0.000 0.000 0.020 0.120 0.704
#&gt; GSM28736     6  0.7106     0.0599 0.008 0.068 0.000 0.232 0.260 0.432
#&gt; GSM28737     1  0.1196     0.7390 0.952 0.000 0.000 0.000 0.008 0.040
#&gt; GSM11249     3  0.2982     0.7740 0.012 0.000 0.828 0.000 0.008 0.152
#&gt; GSM28745     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0146     0.7520 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM28739     1  0.0891     0.7367 0.968 0.000 0.008 0.000 0.000 0.024
#&gt; GSM11243     3  0.3336     0.7946 0.016 0.000 0.808 0.000 0.016 0.160
#&gt; GSM28740     1  0.0000     0.7524 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     6  0.5760     0.4226 0.224 0.000 0.000 0.000 0.268 0.508
#&gt; GSM28726     5  0.5741     0.2799 0.160 0.000 0.000 0.028 0.600 0.212
#&gt; GSM28743     1  0.0972     0.7479 0.964 0.000 0.000 0.008 0.000 0.028
#&gt; GSM11256     4  0.0458     0.8423 0.000 0.000 0.000 0.984 0.016 0.000
#&gt; GSM11262     1  0.0820     0.7436 0.972 0.000 0.000 0.016 0.000 0.012
#&gt; GSM28724     6  0.5073     0.4843 0.220 0.000 0.016 0.000 0.104 0.660
#&gt; GSM28725     3  0.0603     0.8464 0.000 0.000 0.980 0.000 0.004 0.016
#&gt; GSM11263     3  0.0000     0.8469 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0363     0.8454 0.000 0.000 0.988 0.000 0.000 0.012
#&gt; GSM28744     4  0.0146     0.8512 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM28734     4  0.0547     0.8512 0.020 0.000 0.000 0.980 0.000 0.000
#&gt; GSM28747     6  0.4847     0.4331 0.340 0.000 0.000 0.000 0.072 0.588
#&gt; GSM11257     5  0.4265     0.1001 0.004 0.000 0.000 0.384 0.596 0.016
#&gt; GSM11252     6  0.5817     0.1603 0.300 0.000 0.080 0.004 0.044 0.572
#&gt; GSM11264     3  0.1719     0.8375 0.000 0.000 0.924 0.000 0.060 0.016
#&gt; GSM11247     3  0.4278     0.7329 0.020 0.000 0.716 0.000 0.032 0.232
#&gt; GSM11258     4  0.3446     0.5811 0.308 0.000 0.000 0.692 0.000 0.000
#&gt; GSM28728     6  0.6077     0.2233 0.248 0.000 0.004 0.000 0.300 0.448
#&gt; GSM28746     1  0.6193    -0.1301 0.472 0.000 0.004 0.008 0.224 0.292
#&gt; GSM28738     5  0.0665     0.5685 0.008 0.000 0.000 0.004 0.980 0.008
#&gt; GSM28741     2  0.4211     0.3513 0.004 0.616 0.000 0.000 0.016 0.364
#&gt; GSM28729     5  0.3013     0.5685 0.068 0.000 0.000 0.000 0.844 0.088
#&gt; GSM28742     5  0.2482     0.5459 0.000 0.000 0.000 0.004 0.848 0.148
#&gt; GSM11250     2  0.0000     0.9557 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11245     6  0.6502     0.1704 0.208 0.000 0.256 0.004 0.036 0.496
#&gt; GSM11246     1  0.1049     0.7416 0.960 0.000 0.000 0.000 0.008 0.032
#&gt; GSM11261     3  0.3803     0.7137 0.020 0.000 0.724 0.000 0.004 0.252
#&gt; GSM11248     3  0.3963     0.7257 0.008 0.000 0.756 0.000 0.048 0.188
#&gt; GSM28732     6  0.5276     0.4934 0.208 0.000 0.000 0.000 0.188 0.604
#&gt; GSM11255     1  0.6586     0.0285 0.420 0.000 0.056 0.000 0.152 0.372
#&gt; GSM28731     5  0.5759    -0.0132 0.392 0.000 0.000 0.000 0.436 0.172
#&gt; GSM28727     6  0.5276     0.5030 0.312 0.000 0.000 0.000 0.124 0.564
#&gt; GSM11251     1  0.4843     0.1244 0.616 0.000 0.000 0.000 0.084 0.300
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> CV:NMF 50     0.394 2
#> CV:NMF 48     0.423 3
#> CV:NMF 49     0.436 4
#> CV:NMF 38     0.486 5
#> CV:NMF 35     0.418 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.970       0.987         0.3439 0.650   0.650
#> 3 3 0.835           0.864       0.944         0.5830 0.838   0.751
#> 4 4 0.771           0.826       0.909         0.2694 0.800   0.594
#> 5 5 0.808           0.885       0.910         0.0567 0.975   0.916
#> 6 6 0.785           0.809       0.873         0.0485 0.996   0.985
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1   0.163      0.972 0.976 0.024
#&gt; GSM28736     1   0.163      0.972 0.976 0.024
#&gt; GSM28737     1   0.000      0.994 1.000 0.000
#&gt; GSM11249     1   0.000      0.994 1.000 0.000
#&gt; GSM28745     2   0.000      0.953 0.000 1.000
#&gt; GSM11244     2   0.000      0.953 0.000 1.000
#&gt; GSM28748     2   0.000      0.953 0.000 1.000
#&gt; GSM11266     2   0.000      0.953 0.000 1.000
#&gt; GSM28730     2   0.000      0.953 0.000 1.000
#&gt; GSM11253     2   0.000      0.953 0.000 1.000
#&gt; GSM11254     2   0.000      0.953 0.000 1.000
#&gt; GSM11260     2   0.000      0.953 0.000 1.000
#&gt; GSM28733     2   0.000      0.953 0.000 1.000
#&gt; GSM11265     1   0.000      0.994 1.000 0.000
#&gt; GSM28739     1   0.000      0.994 1.000 0.000
#&gt; GSM11243     1   0.000      0.994 1.000 0.000
#&gt; GSM28740     1   0.000      0.994 1.000 0.000
#&gt; GSM11259     1   0.000      0.994 1.000 0.000
#&gt; GSM28726     1   0.000      0.994 1.000 0.000
#&gt; GSM28743     1   0.000      0.994 1.000 0.000
#&gt; GSM11256     1   0.000      0.994 1.000 0.000
#&gt; GSM11262     1   0.000      0.994 1.000 0.000
#&gt; GSM28724     1   0.000      0.994 1.000 0.000
#&gt; GSM28725     1   0.000      0.994 1.000 0.000
#&gt; GSM11263     1   0.000      0.994 1.000 0.000
#&gt; GSM11267     1   0.000      0.994 1.000 0.000
#&gt; GSM28744     1   0.000      0.994 1.000 0.000
#&gt; GSM28734     1   0.000      0.994 1.000 0.000
#&gt; GSM28747     1   0.000      0.994 1.000 0.000
#&gt; GSM11257     1   0.000      0.994 1.000 0.000
#&gt; GSM11252     1   0.000      0.994 1.000 0.000
#&gt; GSM11264     1   0.000      0.994 1.000 0.000
#&gt; GSM11247     1   0.000      0.994 1.000 0.000
#&gt; GSM11258     1   0.000      0.994 1.000 0.000
#&gt; GSM28728     1   0.000      0.994 1.000 0.000
#&gt; GSM28746     1   0.000      0.994 1.000 0.000
#&gt; GSM28738     1   0.000      0.994 1.000 0.000
#&gt; GSM28741     2   0.913      0.528 0.328 0.672
#&gt; GSM28729     1   0.000      0.994 1.000 0.000
#&gt; GSM28742     1   0.000      0.994 1.000 0.000
#&gt; GSM11250     2   0.574      0.833 0.136 0.864
#&gt; GSM11245     1   0.000      0.994 1.000 0.000
#&gt; GSM11246     1   0.000      0.994 1.000 0.000
#&gt; GSM11261     1   0.615      0.812 0.848 0.152
#&gt; GSM11248     1   0.000      0.994 1.000 0.000
#&gt; GSM28732     1   0.000      0.994 1.000 0.000
#&gt; GSM11255     1   0.000      0.994 1.000 0.000
#&gt; GSM28731     1   0.000      0.994 1.000 0.000
#&gt; GSM28727     1   0.000      0.994 1.000 0.000
#&gt; GSM11251     1   0.000      0.994 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1   0.103      0.904 0.976 0.024 0.000
#&gt; GSM28736     1   0.103      0.904 0.976 0.024 0.000
#&gt; GSM28737     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11249     1   0.608      0.458 0.612 0.000 0.388
#&gt; GSM28745     2   0.000      0.928 0.000 1.000 0.000
#&gt; GSM11244     2   0.000      0.928 0.000 1.000 0.000
#&gt; GSM28748     2   0.000      0.928 0.000 1.000 0.000
#&gt; GSM11266     2   0.000      0.928 0.000 1.000 0.000
#&gt; GSM28730     2   0.000      0.928 0.000 1.000 0.000
#&gt; GSM11253     2   0.000      0.928 0.000 1.000 0.000
#&gt; GSM11254     2   0.000      0.928 0.000 1.000 0.000
#&gt; GSM11260     2   0.000      0.928 0.000 1.000 0.000
#&gt; GSM28733     2   0.000      0.928 0.000 1.000 0.000
#&gt; GSM11265     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28739     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11243     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28740     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11259     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28726     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28743     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11256     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11262     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28724     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28725     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11263     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11267     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28744     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28734     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28747     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11257     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11252     1   0.586      0.540 0.656 0.000 0.344
#&gt; GSM11264     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11247     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11258     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28728     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28746     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28738     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28741     2   0.576      0.475 0.328 0.672 0.000
#&gt; GSM28729     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28742     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11250     2   0.362      0.766 0.136 0.864 0.000
#&gt; GSM11245     1   0.586      0.540 0.656 0.000 0.344
#&gt; GSM11246     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11261     1   0.919      0.149 0.468 0.152 0.380
#&gt; GSM11248     1   0.608      0.458 0.612 0.000 0.388
#&gt; GSM28732     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11255     1   0.559      0.602 0.696 0.000 0.304
#&gt; GSM28731     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM28727     1   0.000      0.923 1.000 0.000 0.000
#&gt; GSM11251     1   0.000      0.923 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.1406      0.923 0.960 0.024 0.000 0.016
#&gt; GSM28736     1  0.1406      0.923 0.960 0.024 0.000 0.016
#&gt; GSM28737     1  0.2469      0.911 0.892 0.000 0.000 0.108
#&gt; GSM11249     4  0.4817      0.562 0.000 0.000 0.388 0.612
#&gt; GSM28745     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.938 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.2469      0.911 0.892 0.000 0.000 0.108
#&gt; GSM28739     1  0.2469      0.911 0.892 0.000 0.000 0.108
#&gt; GSM11243     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM28740     1  0.2469      0.911 0.892 0.000 0.000 0.108
#&gt; GSM11259     1  0.0000      0.940 1.000 0.000 0.000 0.000
#&gt; GSM28726     1  0.0592      0.936 0.984 0.000 0.000 0.016
#&gt; GSM28743     1  0.2469      0.911 0.892 0.000 0.000 0.108
#&gt; GSM11256     4  0.1302      0.658 0.044 0.000 0.000 0.956
#&gt; GSM11262     1  0.2469      0.911 0.892 0.000 0.000 0.108
#&gt; GSM28724     1  0.0000      0.940 1.000 0.000 0.000 0.000
#&gt; GSM28725     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.1302      0.658 0.044 0.000 0.000 0.956
#&gt; GSM28734     4  0.0592      0.658 0.016 0.000 0.000 0.984
#&gt; GSM28747     1  0.1118      0.937 0.964 0.000 0.000 0.036
#&gt; GSM11257     1  0.0336      0.939 0.992 0.000 0.000 0.008
#&gt; GSM11252     4  0.5807      0.606 0.044 0.000 0.344 0.612
#&gt; GSM11264     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0000      0.870 0.000 0.000 1.000 0.000
#&gt; GSM11258     4  0.2281      0.633 0.096 0.000 0.000 0.904
#&gt; GSM28728     1  0.0592      0.936 0.984 0.000 0.000 0.016
#&gt; GSM28746     1  0.3356      0.842 0.824 0.000 0.000 0.176
#&gt; GSM28738     1  0.0336      0.939 0.992 0.000 0.000 0.008
#&gt; GSM28741     2  0.5026      0.535 0.312 0.672 0.000 0.016
#&gt; GSM28729     1  0.1118      0.940 0.964 0.000 0.000 0.036
#&gt; GSM28742     1  0.0592      0.936 0.984 0.000 0.000 0.016
#&gt; GSM11250     2  0.3224      0.795 0.120 0.864 0.000 0.016
#&gt; GSM11245     4  0.5807      0.606 0.044 0.000 0.344 0.612
#&gt; GSM11246     1  0.2469      0.911 0.892 0.000 0.000 0.108
#&gt; GSM11261     3  0.8994     -0.373 0.096 0.152 0.380 0.372
#&gt; GSM11248     4  0.4817      0.562 0.000 0.000 0.388 0.612
#&gt; GSM28732     1  0.0336      0.941 0.992 0.000 0.000 0.008
#&gt; GSM11255     4  0.7359      0.477 0.188 0.000 0.304 0.508
#&gt; GSM28731     1  0.1389      0.934 0.952 0.000 0.000 0.048
#&gt; GSM28727     1  0.0000      0.940 1.000 0.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.940 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     1  0.2674      0.838 0.868 0.000 0.000 0.120 0.012
#&gt; GSM28736     1  0.2674      0.838 0.868 0.000 0.000 0.120 0.012
#&gt; GSM28737     1  0.3255      0.881 0.848 0.000 0.000 0.052 0.100
#&gt; GSM11249     5  0.1043      0.815 0.000 0.000 0.040 0.000 0.960
#&gt; GSM28745     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.943 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3255      0.881 0.848 0.000 0.000 0.052 0.100
#&gt; GSM28739     1  0.3255      0.881 0.848 0.000 0.000 0.052 0.100
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28740     1  0.3255      0.881 0.848 0.000 0.000 0.052 0.100
#&gt; GSM11259     1  0.0290      0.907 0.992 0.000 0.000 0.008 0.000
#&gt; GSM28726     1  0.1774      0.893 0.932 0.000 0.000 0.052 0.016
#&gt; GSM28743     1  0.3255      0.881 0.848 0.000 0.000 0.052 0.100
#&gt; GSM11256     4  0.3093      0.899 0.008 0.000 0.000 0.824 0.168
#&gt; GSM11262     1  0.3255      0.881 0.848 0.000 0.000 0.052 0.100
#&gt; GSM28724     1  0.0963      0.902 0.964 0.000 0.000 0.036 0.000
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     4  0.3093      0.899 0.008 0.000 0.000 0.824 0.168
#&gt; GSM28734     4  0.3231      0.888 0.004 0.000 0.000 0.800 0.196
#&gt; GSM28747     1  0.1493      0.909 0.948 0.000 0.000 0.024 0.028
#&gt; GSM11257     1  0.1469      0.909 0.948 0.000 0.000 0.036 0.016
#&gt; GSM11252     5  0.1997      0.832 0.040 0.000 0.036 0.000 0.924
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11258     4  0.4763      0.736 0.076 0.000 0.000 0.712 0.212
#&gt; GSM28728     1  0.1809      0.886 0.928 0.000 0.000 0.060 0.012
#&gt; GSM28746     1  0.3861      0.845 0.804 0.000 0.000 0.068 0.128
#&gt; GSM28738     1  0.1469      0.909 0.948 0.000 0.000 0.036 0.016
#&gt; GSM28741     2  0.5727      0.543 0.220 0.648 0.000 0.120 0.012
#&gt; GSM28729     1  0.2645      0.903 0.888 0.000 0.000 0.068 0.044
#&gt; GSM28742     1  0.1774      0.893 0.932 0.000 0.000 0.052 0.016
#&gt; GSM11250     2  0.3556      0.806 0.044 0.840 0.000 0.104 0.012
#&gt; GSM11245     5  0.1997      0.832 0.040 0.000 0.036 0.000 0.924
#&gt; GSM11246     1  0.3255      0.881 0.848 0.000 0.000 0.052 0.100
#&gt; GSM11261     5  0.6150      0.618 0.040 0.128 0.040 0.092 0.700
#&gt; GSM11248     5  0.1043      0.815 0.000 0.000 0.040 0.000 0.960
#&gt; GSM28732     1  0.0912      0.910 0.972 0.000 0.000 0.016 0.012
#&gt; GSM11255     5  0.4036      0.651 0.132 0.000 0.012 0.052 0.804
#&gt; GSM28731     1  0.2520      0.901 0.896 0.000 0.000 0.056 0.048
#&gt; GSM28727     1  0.0404      0.906 0.988 0.000 0.000 0.012 0.000
#&gt; GSM11251     1  0.0404      0.906 0.988 0.000 0.000 0.012 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     1  0.3101      0.674 0.756 0.000 0.000 0.000 0.244 0.000
#&gt; GSM28736     1  0.3101      0.674 0.756 0.000 0.000 0.000 0.244 0.000
#&gt; GSM28737     1  0.3337      0.773 0.736 0.000 0.000 0.000 0.260 0.004
#&gt; GSM11249     6  0.0291      0.764 0.000 0.000 0.004 0.004 0.000 0.992
#&gt; GSM28745     2  0.0000      0.939 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.939 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.939 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.939 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.939 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.939 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.939 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.939 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.939 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3337      0.773 0.736 0.000 0.000 0.000 0.260 0.004
#&gt; GSM28739     1  0.3337      0.773 0.736 0.000 0.000 0.000 0.260 0.004
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28740     1  0.3337      0.773 0.736 0.000 0.000 0.000 0.260 0.004
#&gt; GSM11259     1  0.0146      0.813 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM28726     1  0.2668      0.754 0.828 0.000 0.000 0.000 0.168 0.004
#&gt; GSM28743     1  0.3337      0.773 0.736 0.000 0.000 0.000 0.260 0.004
#&gt; GSM11256     4  0.0260      0.893 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM11262     1  0.3337      0.773 0.736 0.000 0.000 0.000 0.260 0.004
#&gt; GSM28724     1  0.1556      0.809 0.920 0.000 0.000 0.000 0.080 0.000
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0260      0.893 0.000 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28734     4  0.1257      0.886 0.000 0.000 0.000 0.952 0.020 0.028
#&gt; GSM28747     1  0.1714      0.817 0.908 0.000 0.000 0.000 0.092 0.000
#&gt; GSM11257     1  0.3230      0.771 0.776 0.000 0.000 0.012 0.212 0.000
#&gt; GSM11252     6  0.1226      0.793 0.040 0.000 0.000 0.004 0.004 0.952
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11258     4  0.3572      0.730 0.060 0.000 0.000 0.820 0.100 0.020
#&gt; GSM28728     1  0.2416      0.756 0.844 0.000 0.000 0.000 0.156 0.000
#&gt; GSM28746     1  0.4619      0.751 0.712 0.000 0.000 0.056 0.204 0.028
#&gt; GSM28738     1  0.3230      0.771 0.776 0.000 0.000 0.012 0.212 0.000
#&gt; GSM28741     2  0.4969      0.473 0.156 0.648 0.000 0.000 0.196 0.000
#&gt; GSM28729     1  0.3398      0.782 0.768 0.000 0.000 0.012 0.216 0.004
#&gt; GSM28742     1  0.2668      0.754 0.828 0.000 0.000 0.000 0.168 0.004
#&gt; GSM11250     2  0.2558      0.782 0.004 0.840 0.000 0.000 0.156 0.000
#&gt; GSM11245     6  0.1226      0.793 0.040 0.000 0.000 0.004 0.004 0.952
#&gt; GSM11246     1  0.3337      0.773 0.736 0.000 0.000 0.000 0.260 0.004
#&gt; GSM11261     5  0.5304      0.000 0.024 0.044 0.004 0.000 0.536 0.392
#&gt; GSM11248     6  0.0291      0.764 0.000 0.000 0.004 0.004 0.000 0.992
#&gt; GSM28732     1  0.1411      0.818 0.936 0.000 0.000 0.004 0.060 0.000
#&gt; GSM11255     6  0.4272      0.433 0.080 0.000 0.000 0.012 0.160 0.748
#&gt; GSM28731     1  0.2982      0.806 0.820 0.000 0.000 0.012 0.164 0.004
#&gt; GSM28727     1  0.0260      0.812 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM11251     1  0.0260      0.812 0.992 0.000 0.000 0.000 0.008 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:hclust 50     0.394 2
#> MAD:hclust 46     0.364 3
#> MAD:hclust 48     0.437 4
#> MAD:hclust 50     0.473 5
#> MAD:hclust 47     0.463 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.393           0.280       0.614         0.3639 0.556   0.556
#> 3 3 1.000           0.986       0.987         0.5228 0.616   0.445
#> 4 4 0.700           0.712       0.808         0.2712 0.804   0.560
#> 5 5 0.754           0.735       0.860         0.0996 0.951   0.814
#> 6 6 0.797           0.644       0.813         0.0522 0.943   0.759
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     2   0.980     0.3074 0.416 0.584
#&gt; GSM28736     2   0.980     0.3074 0.416 0.584
#&gt; GSM28737     2   0.980     0.3074 0.416 0.584
#&gt; GSM11249     1   0.760     0.4468 0.780 0.220
#&gt; GSM28745     2   0.760     0.2347 0.220 0.780
#&gt; GSM11244     2   0.760     0.2347 0.220 0.780
#&gt; GSM28748     2   0.760     0.2347 0.220 0.780
#&gt; GSM11266     2   0.760     0.2347 0.220 0.780
#&gt; GSM28730     2   0.760     0.2347 0.220 0.780
#&gt; GSM11253     2   0.760     0.2347 0.220 0.780
#&gt; GSM11254     2   0.760     0.2347 0.220 0.780
#&gt; GSM11260     2   0.760     0.2347 0.220 0.780
#&gt; GSM28733     2   0.760     0.2347 0.220 0.780
#&gt; GSM11265     2   0.980     0.3074 0.416 0.584
#&gt; GSM28739     2   0.980     0.3074 0.416 0.584
#&gt; GSM11243     1   0.000     0.4942 1.000 0.000
#&gt; GSM28740     2   0.980     0.3074 0.416 0.584
#&gt; GSM11259     2   0.980     0.3074 0.416 0.584
#&gt; GSM28726     2   0.980     0.3074 0.416 0.584
#&gt; GSM28743     2   0.980     0.3074 0.416 0.584
#&gt; GSM11256     1   1.000     0.0780 0.504 0.496
#&gt; GSM11262     2   0.980     0.3074 0.416 0.584
#&gt; GSM28724     2   0.980     0.3074 0.416 0.584
#&gt; GSM28725     1   0.000     0.4942 1.000 0.000
#&gt; GSM11263     1   0.000     0.4942 1.000 0.000
#&gt; GSM11267     1   0.000     0.4942 1.000 0.000
#&gt; GSM28744     1   1.000     0.0780 0.504 0.496
#&gt; GSM28734     1   0.996     0.1598 0.536 0.464
#&gt; GSM28747     2   0.980     0.3074 0.416 0.584
#&gt; GSM11257     2   0.980     0.3074 0.416 0.584
#&gt; GSM11252     1   1.000     0.0945 0.508 0.492
#&gt; GSM11264     1   0.000     0.4942 1.000 0.000
#&gt; GSM11247     1   0.000     0.4942 1.000 0.000
#&gt; GSM11258     1   1.000     0.0572 0.500 0.500
#&gt; GSM28728     2   0.980     0.3074 0.416 0.584
#&gt; GSM28746     2   1.000    -0.0733 0.488 0.512
#&gt; GSM28738     2   0.980     0.3074 0.416 0.584
#&gt; GSM28741     2   0.184     0.2095 0.028 0.972
#&gt; GSM28729     2   0.980     0.3074 0.416 0.584
#&gt; GSM28742     2   0.980     0.3074 0.416 0.584
#&gt; GSM11250     2   0.760     0.2347 0.220 0.780
#&gt; GSM11245     1   1.000     0.0945 0.508 0.492
#&gt; GSM11246     2   0.980     0.3074 0.416 0.584
#&gt; GSM11261     1   0.833     0.2249 0.736 0.264
#&gt; GSM11248     1   0.760     0.4468 0.780 0.220
#&gt; GSM28732     2   0.980     0.3074 0.416 0.584
#&gt; GSM11255     1   1.000     0.0945 0.508 0.492
#&gt; GSM28731     2   0.980     0.3074 0.416 0.584
#&gt; GSM28727     2   0.980     0.3074 0.416 0.584
#&gt; GSM11251     2   0.980     0.3074 0.416 0.584
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28736     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28737     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11249     3  0.0237      0.990 0.004 0.000 0.996
#&gt; GSM28745     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM11244     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM28748     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM11266     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM28730     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM11253     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM11254     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM11260     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM28733     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM11265     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM28739     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM11243     3  0.0829      0.989 0.004 0.012 0.984
#&gt; GSM28740     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM11259     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28726     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28743     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM11256     1  0.1015      0.980 0.980 0.012 0.008
#&gt; GSM11262     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM28724     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28725     3  0.0475      0.991 0.004 0.004 0.992
#&gt; GSM11263     3  0.0475      0.991 0.004 0.004 0.992
#&gt; GSM11267     3  0.0475      0.991 0.004 0.004 0.992
#&gt; GSM28744     1  0.1015      0.980 0.980 0.012 0.008
#&gt; GSM28734     1  0.1015      0.980 0.980 0.012 0.008
#&gt; GSM28747     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11257     1  0.0237      0.988 0.996 0.000 0.004
#&gt; GSM11252     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM11264     3  0.0475      0.991 0.004 0.004 0.992
#&gt; GSM11247     3  0.0829      0.989 0.004 0.012 0.984
#&gt; GSM11258     1  0.1015      0.980 0.980 0.012 0.008
#&gt; GSM28728     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28746     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM28738     1  0.0237      0.988 0.996 0.000 0.004
#&gt; GSM28741     1  0.4399      0.764 0.812 0.188 0.000
#&gt; GSM28729     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28742     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11250     2  0.0892      1.000 0.020 0.980 0.000
#&gt; GSM11245     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM11246     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM11261     3  0.1999      0.952 0.036 0.012 0.952
#&gt; GSM11248     3  0.0237      0.990 0.004 0.000 0.996
#&gt; GSM28732     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11255     1  0.0237      0.989 0.996 0.000 0.004
#&gt; GSM28731     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.990 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.0592      0.807 0.984 0.000 0.000 0.016
#&gt; GSM28736     1  0.0592      0.807 0.984 0.000 0.000 0.016
#&gt; GSM28737     4  0.4972      0.504 0.456 0.000 0.000 0.544
#&gt; GSM11249     3  0.2868      0.858 0.000 0.000 0.864 0.136
#&gt; GSM28745     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11244     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM28748     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11266     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM28730     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11253     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11254     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11260     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM28733     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11265     4  0.4916      0.567 0.424 0.000 0.000 0.576
#&gt; GSM28739     4  0.4916      0.567 0.424 0.000 0.000 0.576
#&gt; GSM11243     3  0.1635      0.901 0.000 0.008 0.948 0.044
#&gt; GSM28740     4  0.4916      0.567 0.424 0.000 0.000 0.576
#&gt; GSM11259     1  0.1867      0.779 0.928 0.000 0.000 0.072
#&gt; GSM28726     1  0.0000      0.816 1.000 0.000 0.000 0.000
#&gt; GSM28743     4  0.4916      0.567 0.424 0.000 0.000 0.576
#&gt; GSM11256     4  0.4998      0.065 0.488 0.000 0.000 0.512
#&gt; GSM11262     4  0.4916      0.567 0.424 0.000 0.000 0.576
#&gt; GSM28724     1  0.1557      0.793 0.944 0.000 0.000 0.056
#&gt; GSM28725     3  0.0188      0.910 0.000 0.004 0.996 0.000
#&gt; GSM11263     3  0.0000      0.910 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      0.910 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.4998      0.065 0.488 0.000 0.000 0.512
#&gt; GSM28734     4  0.4431      0.289 0.304 0.000 0.000 0.696
#&gt; GSM28747     1  0.4250      0.396 0.724 0.000 0.000 0.276
#&gt; GSM11257     1  0.2216      0.710 0.908 0.000 0.000 0.092
#&gt; GSM11252     4  0.4888      0.434 0.412 0.000 0.000 0.588
#&gt; GSM11264     3  0.0000      0.910 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.1635      0.901 0.000 0.008 0.948 0.044
#&gt; GSM11258     4  0.1389      0.448 0.048 0.000 0.000 0.952
#&gt; GSM28728     1  0.0000      0.816 1.000 0.000 0.000 0.000
#&gt; GSM28746     4  0.4999      0.384 0.492 0.000 0.000 0.508
#&gt; GSM28738     1  0.0592      0.804 0.984 0.000 0.000 0.016
#&gt; GSM28741     1  0.0707      0.802 0.980 0.020 0.000 0.000
#&gt; GSM28729     1  0.0000      0.816 1.000 0.000 0.000 0.000
#&gt; GSM28742     1  0.0000      0.816 1.000 0.000 0.000 0.000
#&gt; GSM11250     2  0.0336      1.000 0.008 0.992 0.000 0.000
#&gt; GSM11245     4  0.4888      0.434 0.412 0.000 0.000 0.588
#&gt; GSM11246     4  0.4916      0.567 0.424 0.000 0.000 0.576
#&gt; GSM11261     3  0.7041      0.515 0.304 0.004 0.560 0.132
#&gt; GSM11248     3  0.2868      0.858 0.000 0.000 0.864 0.136
#&gt; GSM28732     1  0.1637      0.789 0.940 0.000 0.000 0.060
#&gt; GSM11255     4  0.4406      0.560 0.300 0.000 0.000 0.700
#&gt; GSM28731     1  0.4103      0.454 0.744 0.000 0.000 0.256
#&gt; GSM28727     1  0.4008      0.484 0.756 0.000 0.000 0.244
#&gt; GSM11251     1  0.4134      0.448 0.740 0.000 0.000 0.260
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.1270      0.776 0.000 0.000 0.000 0.052 0.948
#&gt; GSM28736     5  0.1270      0.776 0.000 0.000 0.000 0.052 0.948
#&gt; GSM28737     1  0.2516      0.785 0.860 0.000 0.000 0.000 0.140
#&gt; GSM11249     3  0.4272      0.714 0.052 0.000 0.752 0.196 0.000
#&gt; GSM28745     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0290      0.993 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11266     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.2377      0.794 0.872 0.000 0.000 0.000 0.128
#&gt; GSM28739     1  0.2377      0.794 0.872 0.000 0.000 0.000 0.128
#&gt; GSM11243     3  0.2304      0.867 0.044 0.000 0.908 0.048 0.000
#&gt; GSM28740     1  0.2377      0.794 0.872 0.000 0.000 0.000 0.128
#&gt; GSM11259     5  0.0955      0.778 0.028 0.000 0.000 0.004 0.968
#&gt; GSM28726     5  0.0794      0.783 0.000 0.000 0.000 0.028 0.972
#&gt; GSM28743     1  0.2377      0.794 0.872 0.000 0.000 0.000 0.128
#&gt; GSM11256     4  0.2228      0.974 0.040 0.000 0.000 0.912 0.048
#&gt; GSM11262     1  0.2377      0.794 0.872 0.000 0.000 0.000 0.128
#&gt; GSM28724     5  0.1124      0.777 0.036 0.000 0.000 0.004 0.960
#&gt; GSM28725     3  0.0000      0.897 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.897 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.897 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     4  0.2228      0.974 0.040 0.000 0.000 0.912 0.048
#&gt; GSM28734     4  0.2300      0.947 0.072 0.000 0.000 0.904 0.024
#&gt; GSM28747     5  0.4367      0.207 0.416 0.000 0.000 0.004 0.580
#&gt; GSM11257     5  0.3885      0.631 0.040 0.000 0.000 0.176 0.784
#&gt; GSM11252     1  0.5989      0.364 0.536 0.000 0.000 0.336 0.128
#&gt; GSM11264     3  0.0000      0.897 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.2376      0.865 0.044 0.000 0.904 0.052 0.000
#&gt; GSM11258     1  0.4287     -0.100 0.540 0.000 0.000 0.460 0.000
#&gt; GSM28728     5  0.0404      0.784 0.012 0.000 0.000 0.000 0.988
#&gt; GSM28746     1  0.6492      0.303 0.456 0.000 0.000 0.196 0.348
#&gt; GSM28738     5  0.1661      0.770 0.036 0.000 0.000 0.024 0.940
#&gt; GSM28741     5  0.0794      0.782 0.000 0.000 0.000 0.028 0.972
#&gt; GSM28729     5  0.0693      0.784 0.012 0.000 0.000 0.008 0.980
#&gt; GSM28742     5  0.0865      0.783 0.004 0.000 0.000 0.024 0.972
#&gt; GSM11250     2  0.0290      0.993 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11245     1  0.5989      0.364 0.536 0.000 0.000 0.336 0.128
#&gt; GSM11246     1  0.2377      0.794 0.872 0.000 0.000 0.000 0.128
#&gt; GSM11261     5  0.7443     -0.179 0.084 0.000 0.364 0.124 0.428
#&gt; GSM11248     3  0.4337      0.710 0.056 0.000 0.748 0.196 0.000
#&gt; GSM28732     5  0.0955      0.778 0.028 0.000 0.000 0.004 0.968
#&gt; GSM11255     1  0.3055      0.706 0.864 0.000 0.000 0.072 0.064
#&gt; GSM28731     5  0.4310      0.261 0.392 0.000 0.000 0.004 0.604
#&gt; GSM28727     5  0.4505      0.270 0.384 0.000 0.000 0.012 0.604
#&gt; GSM11251     5  0.4574      0.218 0.412 0.000 0.000 0.012 0.576
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.2074     0.6341 0.004 0.000 0.000 0.048 0.912 0.036
#&gt; GSM28736     5  0.2074     0.6341 0.004 0.000 0.000 0.048 0.912 0.036
#&gt; GSM28737     1  0.0632     0.7564 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM11249     3  0.5937     0.3508 0.012 0.000 0.520 0.188 0.000 0.280
#&gt; GSM28745     2  0.0000     0.9956 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0260     0.9956 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM28748     2  0.0146     0.9940 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11266     2  0.0260     0.9956 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM28730     2  0.0000     0.9956 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9956 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0260     0.9956 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM11260     2  0.0000     0.9956 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0260     0.9956 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM11265     1  0.0632     0.7564 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM28739     1  0.0632     0.7564 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM11243     3  0.2595     0.7334 0.000 0.000 0.836 0.004 0.000 0.160
#&gt; GSM28740     1  0.0632     0.7564 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM11259     5  0.3512     0.6735 0.032 0.000 0.000 0.000 0.772 0.196
#&gt; GSM28726     5  0.1464     0.6558 0.004 0.000 0.000 0.016 0.944 0.036
#&gt; GSM28743     1  0.0632     0.7564 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM11256     4  0.0790     0.9521 0.000 0.000 0.000 0.968 0.032 0.000
#&gt; GSM11262     1  0.0632     0.7564 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM28724     5  0.4093     0.6442 0.024 0.000 0.000 0.004 0.680 0.292
#&gt; GSM28725     3  0.0000     0.7953 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000     0.7953 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000     0.7953 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0790     0.9521 0.000 0.000 0.000 0.968 0.032 0.000
#&gt; GSM28734     4  0.1370     0.9017 0.012 0.000 0.000 0.948 0.004 0.036
#&gt; GSM28747     1  0.5945    -0.3526 0.392 0.000 0.000 0.000 0.392 0.216
#&gt; GSM11257     5  0.5232     0.5010 0.008 0.000 0.000 0.072 0.508 0.412
#&gt; GSM11252     6  0.6612     0.4186 0.304 0.000 0.000 0.252 0.032 0.412
#&gt; GSM11264     3  0.0000     0.7953 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.2772     0.7223 0.000 0.000 0.816 0.004 0.000 0.180
#&gt; GSM11258     1  0.3852     0.1776 0.612 0.000 0.000 0.384 0.000 0.004
#&gt; GSM28728     5  0.3109     0.6711 0.004 0.000 0.000 0.000 0.772 0.224
#&gt; GSM28746     6  0.7030     0.0288 0.304 0.000 0.000 0.072 0.236 0.388
#&gt; GSM28738     5  0.4337     0.5921 0.008 0.000 0.000 0.016 0.604 0.372
#&gt; GSM28741     5  0.0951     0.6569 0.004 0.000 0.000 0.008 0.968 0.020
#&gt; GSM28729     5  0.3628     0.6589 0.004 0.000 0.000 0.008 0.720 0.268
#&gt; GSM28742     5  0.1320     0.6558 0.000 0.000 0.000 0.016 0.948 0.036
#&gt; GSM11250     2  0.0363     0.9940 0.000 0.988 0.000 0.000 0.000 0.012
#&gt; GSM11245     6  0.6612     0.4186 0.304 0.000 0.000 0.252 0.032 0.412
#&gt; GSM11246     1  0.0632     0.7564 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM11261     6  0.6616     0.0462 0.008 0.000 0.156 0.048 0.300 0.488
#&gt; GSM11248     3  0.5988     0.3227 0.012 0.000 0.504 0.188 0.000 0.296
#&gt; GSM28732     5  0.3394     0.6637 0.012 0.000 0.000 0.000 0.752 0.236
#&gt; GSM11255     1  0.5031    -0.1837 0.528 0.000 0.000 0.064 0.004 0.404
#&gt; GSM28731     5  0.6006     0.2122 0.316 0.000 0.000 0.000 0.428 0.256
#&gt; GSM28727     5  0.5159     0.2385 0.380 0.000 0.000 0.000 0.528 0.092
#&gt; GSM11251     5  0.5123     0.2009 0.408 0.000 0.000 0.000 0.508 0.084
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:kmeans  0        NA 2
#> MAD:kmeans 50     0.370 3
#> MAD:kmeans 39     0.405 4
#> MAD:kmeans 41     0.517 5
#> MAD:kmeans 38     0.509 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.976       0.991         0.4768 0.519   0.519
#> 3 3 0.939           0.949       0.978         0.3534 0.754   0.561
#> 4 4 0.798           0.741       0.898         0.1612 0.797   0.494
#> 5 5 0.824           0.741       0.879         0.0727 0.872   0.550
#> 6 6 0.824           0.639       0.824         0.0387 0.928   0.668
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1   0.000      1.000 1.000 0.000
#&gt; GSM28736     2   0.987      0.239 0.432 0.568
#&gt; GSM28737     1   0.000      1.000 1.000 0.000
#&gt; GSM11249     1   0.000      1.000 1.000 0.000
#&gt; GSM28745     2   0.000      0.976 0.000 1.000
#&gt; GSM11244     2   0.000      0.976 0.000 1.000
#&gt; GSM28748     2   0.000      0.976 0.000 1.000
#&gt; GSM11266     2   0.000      0.976 0.000 1.000
#&gt; GSM28730     2   0.000      0.976 0.000 1.000
#&gt; GSM11253     2   0.000      0.976 0.000 1.000
#&gt; GSM11254     2   0.000      0.976 0.000 1.000
#&gt; GSM11260     2   0.000      0.976 0.000 1.000
#&gt; GSM28733     2   0.000      0.976 0.000 1.000
#&gt; GSM11265     1   0.000      1.000 1.000 0.000
#&gt; GSM28739     1   0.000      1.000 1.000 0.000
#&gt; GSM11243     2   0.000      0.976 0.000 1.000
#&gt; GSM28740     1   0.000      1.000 1.000 0.000
#&gt; GSM11259     1   0.000      1.000 1.000 0.000
#&gt; GSM28726     1   0.000      1.000 1.000 0.000
#&gt; GSM28743     1   0.000      1.000 1.000 0.000
#&gt; GSM11256     1   0.000      1.000 1.000 0.000
#&gt; GSM11262     1   0.000      1.000 1.000 0.000
#&gt; GSM28724     1   0.000      1.000 1.000 0.000
#&gt; GSM28725     2   0.000      0.976 0.000 1.000
#&gt; GSM11263     2   0.000      0.976 0.000 1.000
#&gt; GSM11267     2   0.000      0.976 0.000 1.000
#&gt; GSM28744     1   0.000      1.000 1.000 0.000
#&gt; GSM28734     1   0.000      1.000 1.000 0.000
#&gt; GSM28747     1   0.000      1.000 1.000 0.000
#&gt; GSM11257     1   0.000      1.000 1.000 0.000
#&gt; GSM11252     1   0.000      1.000 1.000 0.000
#&gt; GSM11264     2   0.000      0.976 0.000 1.000
#&gt; GSM11247     2   0.000      0.976 0.000 1.000
#&gt; GSM11258     1   0.000      1.000 1.000 0.000
#&gt; GSM28728     1   0.000      1.000 1.000 0.000
#&gt; GSM28746     1   0.000      1.000 1.000 0.000
#&gt; GSM28738     1   0.000      1.000 1.000 0.000
#&gt; GSM28741     2   0.000      0.976 0.000 1.000
#&gt; GSM28729     1   0.000      1.000 1.000 0.000
#&gt; GSM28742     1   0.000      1.000 1.000 0.000
#&gt; GSM11250     2   0.000      0.976 0.000 1.000
#&gt; GSM11245     1   0.000      1.000 1.000 0.000
#&gt; GSM11246     1   0.000      1.000 1.000 0.000
#&gt; GSM11261     2   0.000      0.976 0.000 1.000
#&gt; GSM11248     1   0.000      1.000 1.000 0.000
#&gt; GSM28732     1   0.000      1.000 1.000 0.000
#&gt; GSM11255     1   0.000      1.000 1.000 0.000
#&gt; GSM28731     1   0.000      1.000 1.000 0.000
#&gt; GSM28727     1   0.000      1.000 1.000 0.000
#&gt; GSM11251     1   0.000      1.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28736     2  0.0424      0.990 0.008 0.992 0.000
#&gt; GSM28737     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM11249     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM28745     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11244     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM28748     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11266     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM28730     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11253     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11254     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11260     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM28733     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11265     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM11243     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM28740     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM11259     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28726     1  0.4399      0.778 0.812 0.188 0.000
#&gt; GSM28743     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM11256     3  0.1643      0.920 0.044 0.000 0.956
#&gt; GSM11262     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28724     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28725     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11263     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11267     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM28744     3  0.1643      0.920 0.044 0.000 0.956
#&gt; GSM28734     3  0.0747      0.936 0.016 0.000 0.984
#&gt; GSM28747     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM11257     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM11252     3  0.6111      0.389 0.396 0.000 0.604
#&gt; GSM11264     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11247     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11258     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28728     1  0.3038      0.875 0.896 0.000 0.104
#&gt; GSM28746     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28738     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28741     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM28729     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28742     1  0.4399      0.778 0.812 0.188 0.000
#&gt; GSM11250     2  0.0000      0.999 0.000 1.000 0.000
#&gt; GSM11245     3  0.3551      0.835 0.132 0.000 0.868
#&gt; GSM11246     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM11261     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11248     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM28732     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM11255     1  0.0237      0.974 0.996 0.000 0.004
#&gt; GSM28731     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.978 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.978 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     4  0.0188    0.76383 0.004 0.000 0.000 0.996
#&gt; GSM28736     4  0.1716    0.72884 0.000 0.064 0.000 0.936
#&gt; GSM28737     1  0.0188    0.78501 0.996 0.000 0.000 0.004
#&gt; GSM11249     3  0.0000    0.93862 0.000 0.000 1.000 0.000
#&gt; GSM28745     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.0000    0.78616 1.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000    0.78616 1.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0000    0.93862 0.000 0.000 1.000 0.000
#&gt; GSM28740     1  0.0000    0.78616 1.000 0.000 0.000 0.000
#&gt; GSM11259     4  0.4941   -0.00528 0.436 0.000 0.000 0.564
#&gt; GSM28726     4  0.0336    0.76395 0.008 0.000 0.000 0.992
#&gt; GSM28743     1  0.0000    0.78616 1.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.5855    0.35848 0.356 0.000 0.044 0.600
#&gt; GSM11262     1  0.0000    0.78616 1.000 0.000 0.000 0.000
#&gt; GSM28724     1  0.4994    0.17789 0.520 0.000 0.000 0.480
#&gt; GSM28725     3  0.0000    0.93862 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000    0.93862 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000    0.93862 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.5943    0.34816 0.360 0.000 0.048 0.592
#&gt; GSM28734     3  0.7240    0.17919 0.400 0.000 0.456 0.144
#&gt; GSM28747     1  0.4661    0.44798 0.652 0.000 0.000 0.348
#&gt; GSM11257     4  0.3801    0.59441 0.220 0.000 0.000 0.780
#&gt; GSM11252     1  0.2737    0.72045 0.888 0.000 0.104 0.008
#&gt; GSM11264     3  0.0000    0.93862 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0000    0.93862 0.000 0.000 1.000 0.000
#&gt; GSM11258     1  0.0188    0.78404 0.996 0.000 0.000 0.004
#&gt; GSM28728     4  0.1406    0.75320 0.024 0.000 0.016 0.960
#&gt; GSM28746     1  0.2216    0.73615 0.908 0.000 0.000 0.092
#&gt; GSM28738     4  0.0188    0.76367 0.004 0.000 0.000 0.996
#&gt; GSM28741     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM28729     4  0.0336    0.76395 0.008 0.000 0.000 0.992
#&gt; GSM28742     4  0.0188    0.76383 0.004 0.000 0.000 0.996
#&gt; GSM11250     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11245     1  0.3972    0.61402 0.788 0.000 0.204 0.008
#&gt; GSM11246     1  0.0000    0.78616 1.000 0.000 0.000 0.000
#&gt; GSM11261     3  0.0000    0.93862 0.000 0.000 1.000 0.000
#&gt; GSM11248     3  0.0000    0.93862 0.000 0.000 1.000 0.000
#&gt; GSM28732     4  0.4817    0.15653 0.388 0.000 0.000 0.612
#&gt; GSM11255     1  0.0188    0.78404 0.996 0.000 0.000 0.004
#&gt; GSM28731     1  0.4916    0.31987 0.576 0.000 0.000 0.424
#&gt; GSM28727     1  0.4961    0.27008 0.552 0.000 0.000 0.448
#&gt; GSM11251     1  0.4948    0.28828 0.560 0.000 0.000 0.440
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.3336     0.5598 0.000 0.000 0.000 0.228 0.772
#&gt; GSM28736     5  0.4325     0.5272 0.000 0.044 0.000 0.220 0.736
#&gt; GSM28737     1  0.0404     0.7932 0.988 0.000 0.000 0.000 0.012
#&gt; GSM11249     3  0.1608     0.9282 0.000 0.000 0.928 0.072 0.000
#&gt; GSM28745     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000     0.7997 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000     0.7997 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0000     0.9846 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28740     1  0.0000     0.7997 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     5  0.3430     0.6428 0.220 0.000 0.000 0.004 0.776
#&gt; GSM28726     5  0.1768     0.6702 0.000 0.004 0.000 0.072 0.924
#&gt; GSM28743     1  0.0000     0.7997 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.0324     0.7601 0.004 0.000 0.000 0.992 0.004
#&gt; GSM11262     1  0.0000     0.7997 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28724     5  0.5862     0.4219 0.344 0.000 0.000 0.112 0.544
#&gt; GSM28725     3  0.0000     0.9846 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000     0.9846 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000     0.9846 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     4  0.0324     0.7601 0.004 0.000 0.000 0.992 0.004
#&gt; GSM28734     4  0.1377     0.7569 0.020 0.000 0.020 0.956 0.004
#&gt; GSM28747     1  0.5022     0.1675 0.620 0.000 0.000 0.048 0.332
#&gt; GSM11257     4  0.4822     0.5478 0.076 0.000 0.000 0.704 0.220
#&gt; GSM11252     4  0.4618     0.4809 0.344 0.000 0.016 0.636 0.004
#&gt; GSM11264     3  0.0000     0.9846 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.0000     0.9846 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11258     1  0.4256     0.0222 0.564 0.000 0.000 0.436 0.000
#&gt; GSM28728     5  0.2568     0.6915 0.048 0.000 0.016 0.032 0.904
#&gt; GSM28746     1  0.6262     0.1626 0.504 0.000 0.000 0.332 0.164
#&gt; GSM28738     5  0.2813     0.6123 0.000 0.000 0.000 0.168 0.832
#&gt; GSM28741     2  0.0510     0.9832 0.000 0.984 0.000 0.000 0.016
#&gt; GSM28729     5  0.1043     0.6794 0.000 0.000 0.000 0.040 0.960
#&gt; GSM28742     5  0.1544     0.6718 0.000 0.000 0.000 0.068 0.932
#&gt; GSM11250     2  0.0000     0.9983 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11245     4  0.4882     0.5020 0.328 0.000 0.032 0.636 0.004
#&gt; GSM11246     1  0.0162     0.7979 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11261     3  0.0000     0.9846 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11248     3  0.1197     0.9512 0.000 0.000 0.952 0.048 0.000
#&gt; GSM28732     5  0.2843     0.6880 0.144 0.000 0.000 0.008 0.848
#&gt; GSM11255     1  0.3579     0.5013 0.756 0.000 0.000 0.240 0.004
#&gt; GSM28731     5  0.4882     0.3128 0.444 0.000 0.000 0.024 0.532
#&gt; GSM28727     5  0.4283     0.3418 0.456 0.000 0.000 0.000 0.544
#&gt; GSM11251     5  0.4306     0.2581 0.492 0.000 0.000 0.000 0.508
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.3707     0.6111 0.000 0.000 0.000 0.136 0.784 0.080
#&gt; GSM28736     5  0.3856     0.6159 0.000 0.012 0.000 0.132 0.788 0.068
#&gt; GSM28737     1  0.0405     0.7072 0.988 0.000 0.000 0.000 0.004 0.008
#&gt; GSM11249     3  0.3920     0.7520 0.000 0.000 0.768 0.112 0.000 0.120
#&gt; GSM28745     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000     0.7144 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000     0.7144 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0000     0.9434 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28740     1  0.0000     0.7144 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     6  0.5349     0.5044 0.144 0.000 0.000 0.004 0.256 0.596
#&gt; GSM28726     5  0.1075     0.6316 0.000 0.000 0.000 0.000 0.952 0.048
#&gt; GSM28743     1  0.0000     0.7144 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.0891     0.6576 0.000 0.000 0.000 0.968 0.024 0.008
#&gt; GSM11262     1  0.0000     0.7144 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28724     6  0.6228     0.4794 0.188 0.000 0.000 0.076 0.156 0.580
#&gt; GSM28725     3  0.0000     0.9434 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000     0.9434 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000     0.9434 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0622     0.6648 0.000 0.000 0.000 0.980 0.012 0.008
#&gt; GSM28734     4  0.0436     0.6669 0.000 0.000 0.004 0.988 0.004 0.004
#&gt; GSM28747     1  0.6301    -0.1298 0.436 0.000 0.000 0.048 0.120 0.396
#&gt; GSM11257     4  0.6035     0.2373 0.020 0.000 0.000 0.528 0.188 0.264
#&gt; GSM11252     4  0.6105     0.4902 0.200 0.000 0.008 0.488 0.004 0.300
#&gt; GSM11264     3  0.0000     0.9434 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0000     0.9434 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11258     1  0.3872     0.1927 0.604 0.000 0.000 0.392 0.000 0.004
#&gt; GSM28728     6  0.5418     0.2721 0.024 0.000 0.012 0.048 0.336 0.580
#&gt; GSM28746     6  0.6602     0.1634 0.316 0.000 0.000 0.264 0.028 0.392
#&gt; GSM28738     5  0.5386    -0.0110 0.000 0.000 0.000 0.116 0.496 0.388
#&gt; GSM28741     2  0.1563     0.9285 0.000 0.932 0.000 0.000 0.056 0.012
#&gt; GSM28729     6  0.4250    -0.0516 0.000 0.000 0.000 0.016 0.456 0.528
#&gt; GSM28742     5  0.1714     0.6105 0.000 0.000 0.000 0.000 0.908 0.092
#&gt; GSM11250     2  0.0000     0.9932 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11245     4  0.6320     0.4971 0.180 0.000 0.024 0.488 0.004 0.304
#&gt; GSM11246     1  0.0000     0.7144 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11261     3  0.0405     0.9359 0.000 0.000 0.988 0.004 0.000 0.008
#&gt; GSM11248     3  0.3552     0.7893 0.000 0.000 0.800 0.084 0.000 0.116
#&gt; GSM28732     6  0.4544     0.4532 0.056 0.000 0.000 0.004 0.280 0.660
#&gt; GSM11255     1  0.5067     0.2925 0.612 0.000 0.000 0.120 0.000 0.268
#&gt; GSM28731     6  0.4947     0.5056 0.244 0.000 0.000 0.000 0.120 0.636
#&gt; GSM28727     1  0.6219    -0.2713 0.372 0.000 0.000 0.004 0.284 0.340
#&gt; GSM11251     1  0.5949    -0.1211 0.452 0.000 0.000 0.000 0.248 0.300
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) k
#> MAD:skmeans 49     0.393 2
#> MAD:skmeans 49     0.449 3
#> MAD:skmeans 40     0.406 4
#> MAD:skmeans 42     0.459 5
#> MAD:skmeans 36     0.455 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000          0.351 0.650   0.650
#> 3 3 1.000           1.000       1.000          0.576 0.798   0.688
#> 4 4 1.000           0.994       0.997          0.128 0.931   0.847
#> 5 5 0.851           0.872       0.943          0.108 0.939   0.838
#> 6 6 0.896           0.837       0.938          0.074 0.910   0.736
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2
#&gt; GSM28735     1       0          1  1  0
#&gt; GSM28736     1       0          1  1  0
#&gt; GSM28737     1       0          1  1  0
#&gt; GSM11249     1       0          1  1  0
#&gt; GSM28745     2       0          1  0  1
#&gt; GSM11244     2       0          1  0  1
#&gt; GSM28748     2       0          1  0  1
#&gt; GSM11266     2       0          1  0  1
#&gt; GSM28730     2       0          1  0  1
#&gt; GSM11253     2       0          1  0  1
#&gt; GSM11254     2       0          1  0  1
#&gt; GSM11260     2       0          1  0  1
#&gt; GSM28733     2       0          1  0  1
#&gt; GSM11265     1       0          1  1  0
#&gt; GSM28739     1       0          1  1  0
#&gt; GSM11243     1       0          1  1  0
#&gt; GSM28740     1       0          1  1  0
#&gt; GSM11259     1       0          1  1  0
#&gt; GSM28726     1       0          1  1  0
#&gt; GSM28743     1       0          1  1  0
#&gt; GSM11256     1       0          1  1  0
#&gt; GSM11262     1       0          1  1  0
#&gt; GSM28724     1       0          1  1  0
#&gt; GSM28725     1       0          1  1  0
#&gt; GSM11263     1       0          1  1  0
#&gt; GSM11267     1       0          1  1  0
#&gt; GSM28744     1       0          1  1  0
#&gt; GSM28734     1       0          1  1  0
#&gt; GSM28747     1       0          1  1  0
#&gt; GSM11257     1       0          1  1  0
#&gt; GSM11252     1       0          1  1  0
#&gt; GSM11264     1       0          1  1  0
#&gt; GSM11247     1       0          1  1  0
#&gt; GSM11258     1       0          1  1  0
#&gt; GSM28728     1       0          1  1  0
#&gt; GSM28746     1       0          1  1  0
#&gt; GSM28738     1       0          1  1  0
#&gt; GSM28741     2       0          1  0  1
#&gt; GSM28729     1       0          1  1  0
#&gt; GSM28742     1       0          1  1  0
#&gt; GSM11250     2       0          1  0  1
#&gt; GSM11245     1       0          1  1  0
#&gt; GSM11246     1       0          1  1  0
#&gt; GSM11261     1       0          1  1  0
#&gt; GSM11248     1       0          1  1  0
#&gt; GSM28732     1       0          1  1  0
#&gt; GSM11255     1       0          1  1  0
#&gt; GSM28731     1       0          1  1  0
#&gt; GSM28727     1       0          1  1  0
#&gt; GSM11251     1       0          1  1  0
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2 p3
#&gt; GSM28735     1       0          1  1  0  0
#&gt; GSM28736     1       0          1  1  0  0
#&gt; GSM28737     1       0          1  1  0  0
#&gt; GSM11249     3       0          1  0  0  1
#&gt; GSM28745     2       0          1  0  1  0
#&gt; GSM11244     2       0          1  0  1  0
#&gt; GSM28748     2       0          1  0  1  0
#&gt; GSM11266     2       0          1  0  1  0
#&gt; GSM28730     2       0          1  0  1  0
#&gt; GSM11253     2       0          1  0  1  0
#&gt; GSM11254     2       0          1  0  1  0
#&gt; GSM11260     2       0          1  0  1  0
#&gt; GSM28733     2       0          1  0  1  0
#&gt; GSM11265     1       0          1  1  0  0
#&gt; GSM28739     1       0          1  1  0  0
#&gt; GSM11243     3       0          1  0  0  1
#&gt; GSM28740     1       0          1  1  0  0
#&gt; GSM11259     1       0          1  1  0  0
#&gt; GSM28726     1       0          1  1  0  0
#&gt; GSM28743     1       0          1  1  0  0
#&gt; GSM11256     1       0          1  1  0  0
#&gt; GSM11262     1       0          1  1  0  0
#&gt; GSM28724     1       0          1  1  0  0
#&gt; GSM28725     3       0          1  0  0  1
#&gt; GSM11263     3       0          1  0  0  1
#&gt; GSM11267     3       0          1  0  0  1
#&gt; GSM28744     1       0          1  1  0  0
#&gt; GSM28734     1       0          1  1  0  0
#&gt; GSM28747     1       0          1  1  0  0
#&gt; GSM11257     1       0          1  1  0  0
#&gt; GSM11252     1       0          1  1  0  0
#&gt; GSM11264     3       0          1  0  0  1
#&gt; GSM11247     3       0          1  0  0  1
#&gt; GSM11258     1       0          1  1  0  0
#&gt; GSM28728     1       0          1  1  0  0
#&gt; GSM28746     1       0          1  1  0  0
#&gt; GSM28738     1       0          1  1  0  0
#&gt; GSM28741     2       0          1  0  1  0
#&gt; GSM28729     1       0          1  1  0  0
#&gt; GSM28742     1       0          1  1  0  0
#&gt; GSM11250     2       0          1  0  1  0
#&gt; GSM11245     1       0          1  1  0  0
#&gt; GSM11246     1       0          1  1  0  0
#&gt; GSM11261     1       0          1  1  0  0
#&gt; GSM11248     3       0          1  0  0  1
#&gt; GSM28732     1       0          1  1  0  0
#&gt; GSM11255     1       0          1  1  0  0
#&gt; GSM28731     1       0          1  1  0  0
#&gt; GSM28727     1       0          1  1  0  0
#&gt; GSM11251     1       0          1  1  0  0
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2 p3    p4
#&gt; GSM28735     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28736     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28737     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11249     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM28745     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM11244     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM28748     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM11266     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM28730     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM11253     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM11254     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM11260     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM28733     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM11265     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28739     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11243     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM28740     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11259     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28726     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28743     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11256     4   0.000      1.000 0.000  0  0 1.000
#&gt; GSM11262     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28724     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28725     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM11263     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM11267     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM28744     4   0.000      1.000 0.000  0  0 1.000
#&gt; GSM28734     4   0.000      1.000 0.000  0  0 1.000
#&gt; GSM28747     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11257     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11252     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11264     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM11247     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM11258     1   0.276      0.853 0.872  0  0 0.128
#&gt; GSM28728     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28746     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28738     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28741     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM28729     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28742     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11250     2   0.000      1.000 0.000  1  0 0.000
#&gt; GSM11245     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11246     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11261     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11248     3   0.000      1.000 0.000  0  1 0.000
#&gt; GSM28732     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11255     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28731     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM28727     1   0.000      0.995 1.000  0  0 0.000
#&gt; GSM11251     1   0.000      0.995 1.000  0  0 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2   p3    p4    p5
#&gt; GSM28735     1  0.4219     -0.136 0.584 0.000 0.00 0.000 0.416
#&gt; GSM28736     5  0.3074      0.705 0.196 0.000 0.00 0.000 0.804
#&gt; GSM28737     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM11249     3  0.2516      0.870 0.000 0.000 0.86 0.000 0.140
#&gt; GSM28745     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11244     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM28748     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11266     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM28730     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11253     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11254     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11260     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM28733     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11265     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM28739     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM11243     3  0.0000      0.959 0.000 0.000 1.00 0.000 0.000
#&gt; GSM28740     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM11259     1  0.0609      0.913 0.980 0.000 0.00 0.000 0.020
#&gt; GSM28726     5  0.3074      0.705 0.196 0.000 0.00 0.000 0.804
#&gt; GSM28743     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM11256     4  0.0000      1.000 0.000 0.000 0.00 1.000 0.000
#&gt; GSM11262     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM28724     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM28725     3  0.0000      0.959 0.000 0.000 1.00 0.000 0.000
#&gt; GSM11263     3  0.0000      0.959 0.000 0.000 1.00 0.000 0.000
#&gt; GSM11267     3  0.0000      0.959 0.000 0.000 1.00 0.000 0.000
#&gt; GSM28744     4  0.0000      1.000 0.000 0.000 0.00 1.000 0.000
#&gt; GSM28734     4  0.0000      1.000 0.000 0.000 0.00 1.000 0.000
#&gt; GSM28747     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM11257     1  0.2813      0.774 0.832 0.000 0.00 0.000 0.168
#&gt; GSM11252     1  0.2516      0.790 0.860 0.000 0.00 0.000 0.140
#&gt; GSM11264     3  0.0000      0.959 0.000 0.000 1.00 0.000 0.000
#&gt; GSM11247     3  0.0000      0.959 0.000 0.000 1.00 0.000 0.000
#&gt; GSM11258     1  0.2377      0.794 0.872 0.000 0.00 0.128 0.000
#&gt; GSM28728     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM28746     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM28738     1  0.1732      0.863 0.920 0.000 0.00 0.000 0.080
#&gt; GSM28741     2  0.4201      0.425 0.000 0.592 0.00 0.000 0.408
#&gt; GSM28729     1  0.0510      0.916 0.984 0.000 0.00 0.000 0.016
#&gt; GSM28742     5  0.4305      0.387 0.488 0.000 0.00 0.000 0.512
#&gt; GSM11250     2  0.0000      0.958 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11245     1  0.2516      0.790 0.860 0.000 0.00 0.000 0.140
#&gt; GSM11246     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM11261     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM11248     3  0.2516      0.870 0.000 0.000 0.86 0.000 0.140
#&gt; GSM28732     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM11255     1  0.2329      0.809 0.876 0.000 0.00 0.000 0.124
#&gt; GSM28731     1  0.0000      0.925 1.000 0.000 0.00 0.000 0.000
#&gt; GSM28727     1  0.0609      0.913 0.980 0.000 0.00 0.000 0.020
#&gt; GSM11251     1  0.0609      0.913 0.980 0.000 0.00 0.000 0.020
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.3864      0.326 0.480 0.00 0.000 0.000 0.520 0.000
#&gt; GSM28736     5  0.2491      0.682 0.164 0.00 0.000 0.000 0.836 0.000
#&gt; GSM28737     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11249     6  0.0458      0.978 0.000 0.00 0.016 0.000 0.000 0.984
#&gt; GSM28745     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM28740     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11259     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM28726     5  0.2491      0.682 0.164 0.00 0.000 0.000 0.836 0.000
#&gt; GSM28743     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.0000      1.000 0.000 0.00 0.000 1.000 0.000 0.000
#&gt; GSM11262     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM28724     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM28725     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.0000      1.000 0.000 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28734     4  0.0000      1.000 0.000 0.00 0.000 1.000 0.000 0.000
#&gt; GSM28747     1  0.0146      0.882 0.996 0.00 0.000 0.000 0.000 0.004
#&gt; GSM11257     1  0.5723     -0.119 0.428 0.00 0.000 0.000 0.164 0.408
#&gt; GSM11252     6  0.0458      0.978 0.016 0.00 0.000 0.000 0.000 0.984
#&gt; GSM11264     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11258     1  0.2135      0.740 0.872 0.00 0.000 0.128 0.000 0.000
#&gt; GSM28728     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM28746     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM28738     1  0.3003      0.666 0.812 0.00 0.000 0.000 0.172 0.016
#&gt; GSM28741     2  0.3647      0.515 0.000 0.64 0.000 0.000 0.360 0.000
#&gt; GSM28729     1  0.0146      0.881 0.996 0.00 0.000 0.000 0.004 0.000
#&gt; GSM28742     1  0.5984     -0.430 0.420 0.00 0.000 0.000 0.344 0.236
#&gt; GSM11250     2  0.0000      0.964 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11245     6  0.0458      0.978 0.016 0.00 0.000 0.000 0.000 0.984
#&gt; GSM11246     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11261     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11248     6  0.0458      0.978 0.000 0.00 0.016 0.000 0.000 0.984
#&gt; GSM28732     1  0.0458      0.871 0.984 0.00 0.000 0.000 0.000 0.016
#&gt; GSM11255     1  0.3607      0.354 0.652 0.00 0.000 0.000 0.000 0.348
#&gt; GSM28731     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.885 1.000 0.00 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> MAD:pam 50     0.394 2
#> MAD:pam 50     0.370 3
#> MAD:pam 50     0.560 4
#> MAD:pam 47     0.503 5
#> MAD:pam 46     0.465 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.689           0.818       0.912         0.3577 0.726   0.726
#> 3 3 0.423           0.594       0.783         0.6193 0.653   0.522
#> 4 4 0.607           0.748       0.862         0.1984 0.716   0.396
#> 5 5 0.704           0.795       0.878         0.0964 0.935   0.777
#> 6 6 0.693           0.664       0.806         0.0653 0.927   0.696
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.0672      0.878 0.992 0.008
#&gt; GSM28736     1  0.0672      0.878 0.992 0.008
#&gt; GSM28737     1  0.0000      0.880 1.000 0.000
#&gt; GSM11249     1  0.9580      0.531 0.620 0.380
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000
#&gt; GSM28748     1  0.9866      0.438 0.568 0.432
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000
#&gt; GSM11265     1  0.0000      0.880 1.000 0.000
#&gt; GSM28739     1  0.0000      0.880 1.000 0.000
#&gt; GSM11243     1  0.9580      0.531 0.620 0.380
#&gt; GSM28740     1  0.0000      0.880 1.000 0.000
#&gt; GSM11259     1  0.0000      0.880 1.000 0.000
#&gt; GSM28726     1  0.0672      0.878 0.992 0.008
#&gt; GSM28743     1  0.0000      0.880 1.000 0.000
#&gt; GSM11256     1  0.0672      0.878 0.992 0.008
#&gt; GSM11262     1  0.0000      0.880 1.000 0.000
#&gt; GSM28724     1  0.0000      0.880 1.000 0.000
#&gt; GSM28725     1  0.9580      0.531 0.620 0.380
#&gt; GSM11263     1  0.9580      0.531 0.620 0.380
#&gt; GSM11267     1  0.9580      0.531 0.620 0.380
#&gt; GSM28744     1  0.0672      0.878 0.992 0.008
#&gt; GSM28734     1  0.0672      0.878 0.992 0.008
#&gt; GSM28747     1  0.0000      0.880 1.000 0.000
#&gt; GSM11257     1  0.0672      0.878 0.992 0.008
#&gt; GSM11252     1  0.0000      0.880 1.000 0.000
#&gt; GSM11264     1  0.9580      0.531 0.620 0.380
#&gt; GSM11247     1  0.9580      0.531 0.620 0.380
#&gt; GSM11258     1  0.0672      0.878 0.992 0.008
#&gt; GSM28728     1  0.0000      0.880 1.000 0.000
#&gt; GSM28746     1  0.0000      0.880 1.000 0.000
#&gt; GSM28738     1  0.0672      0.878 0.992 0.008
#&gt; GSM28741     1  0.2603      0.858 0.956 0.044
#&gt; GSM28729     1  0.0672      0.878 0.992 0.008
#&gt; GSM28742     1  0.0672      0.878 0.992 0.008
#&gt; GSM11250     1  0.9866      0.438 0.568 0.432
#&gt; GSM11245     1  0.0000      0.880 1.000 0.000
#&gt; GSM11246     1  0.0000      0.880 1.000 0.000
#&gt; GSM11261     1  0.9580      0.531 0.620 0.380
#&gt; GSM11248     1  0.9580      0.531 0.620 0.380
#&gt; GSM28732     1  0.0000      0.880 1.000 0.000
#&gt; GSM11255     1  0.0000      0.880 1.000 0.000
#&gt; GSM28731     1  0.0000      0.880 1.000 0.000
#&gt; GSM28727     1  0.0000      0.880 1.000 0.000
#&gt; GSM11251     1  0.0000      0.880 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1   0.671   0.206682 0.572 0.012 0.416
#&gt; GSM28736     1   0.844  -0.000943 0.492 0.088 0.420
#&gt; GSM28737     1   0.369   0.651192 0.860 0.000 0.140
#&gt; GSM11249     3   0.432   0.672722 0.112 0.028 0.860
#&gt; GSM28745     2   0.000   1.000000 0.000 1.000 0.000
#&gt; GSM11244     2   0.000   1.000000 0.000 1.000 0.000
#&gt; GSM28748     3   0.912   0.554874 0.220 0.232 0.548
#&gt; GSM11266     2   0.000   1.000000 0.000 1.000 0.000
#&gt; GSM28730     2   0.000   1.000000 0.000 1.000 0.000
#&gt; GSM11253     2   0.000   1.000000 0.000 1.000 0.000
#&gt; GSM11254     2   0.000   1.000000 0.000 1.000 0.000
#&gt; GSM11260     2   0.000   1.000000 0.000 1.000 0.000
#&gt; GSM28733     2   0.000   1.000000 0.000 1.000 0.000
#&gt; GSM11265     1   0.312   0.637739 0.892 0.000 0.108
#&gt; GSM28739     1   0.312   0.637739 0.892 0.000 0.108
#&gt; GSM11243     3   0.116   0.670475 0.000 0.028 0.972
#&gt; GSM28740     1   0.312   0.637739 0.892 0.000 0.108
#&gt; GSM11259     1   0.186   0.673796 0.948 0.000 0.052
#&gt; GSM28726     1   0.802   0.088182 0.520 0.064 0.416
#&gt; GSM28743     1   0.312   0.637739 0.892 0.000 0.108
#&gt; GSM11256     3   0.618   0.407432 0.416 0.000 0.584
#&gt; GSM11262     1   0.312   0.637739 0.892 0.000 0.108
#&gt; GSM28724     1   0.593   0.387487 0.644 0.000 0.356
#&gt; GSM28725     3   0.116   0.670475 0.000 0.028 0.972
#&gt; GSM11263     3   0.116   0.670475 0.000 0.028 0.972
#&gt; GSM11267     3   0.116   0.670475 0.000 0.028 0.972
#&gt; GSM28744     3   0.618   0.407432 0.416 0.000 0.584
#&gt; GSM28734     3   0.562   0.539454 0.308 0.000 0.692
#&gt; GSM28747     1   0.175   0.673902 0.952 0.000 0.048
#&gt; GSM11257     3   0.628   0.263884 0.460 0.000 0.540
#&gt; GSM11252     1   0.540   0.500203 0.720 0.000 0.280
#&gt; GSM11264     3   0.116   0.670475 0.000 0.028 0.972
#&gt; GSM11247     3   0.116   0.670475 0.000 0.028 0.972
#&gt; GSM11258     3   0.623   0.276924 0.436 0.000 0.564
#&gt; GSM28728     1   0.620   0.209360 0.576 0.000 0.424
#&gt; GSM28746     1   0.280   0.666039 0.908 0.000 0.092
#&gt; GSM28738     1   0.709   0.185610 0.560 0.024 0.416
#&gt; GSM28741     3   0.849   0.519225 0.312 0.116 0.572
#&gt; GSM28729     1   0.670   0.218073 0.576 0.012 0.412
#&gt; GSM28742     1   0.709   0.185610 0.560 0.024 0.416
#&gt; GSM11250     3   0.912   0.554874 0.220 0.232 0.548
#&gt; GSM11245     1   0.601   0.428681 0.628 0.000 0.372
#&gt; GSM11246     1   0.312   0.637739 0.892 0.000 0.108
#&gt; GSM11261     3   0.585   0.635731 0.216 0.028 0.756
#&gt; GSM11248     3   0.580   0.638721 0.212 0.028 0.760
#&gt; GSM28732     1   0.424   0.612947 0.824 0.000 0.176
#&gt; GSM11255     1   0.369   0.659181 0.860 0.000 0.140
#&gt; GSM28731     1   0.186   0.673796 0.948 0.000 0.052
#&gt; GSM28727     1   0.175   0.673902 0.952 0.000 0.048
#&gt; GSM11251     1   0.164   0.673177 0.956 0.000 0.044
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     4  0.3172     0.8222 0.160 0.000 0.000 0.840
#&gt; GSM28736     4  0.3172     0.8222 0.160 0.000 0.000 0.840
#&gt; GSM28737     1  0.3074     0.8666 0.848 0.000 0.000 0.152
#&gt; GSM11249     3  0.4730     0.4983 0.000 0.000 0.636 0.364
#&gt; GSM28745     2  0.0000     0.9129 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9129 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.5000    -0.1765 0.000 0.504 0.000 0.496
#&gt; GSM11266     2  0.0000     0.9129 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9129 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9129 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9129 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9129 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9129 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.0000     0.8715 1.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.1211     0.8590 0.960 0.000 0.000 0.040
#&gt; GSM11243     3  0.0000     0.7991 0.000 0.000 1.000 0.000
#&gt; GSM28740     1  0.0000     0.8715 1.000 0.000 0.000 0.000
#&gt; GSM11259     1  0.3356     0.8411 0.824 0.000 0.000 0.176
#&gt; GSM28726     4  0.3400     0.8135 0.180 0.000 0.000 0.820
#&gt; GSM28743     1  0.0000     0.8715 1.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.1929     0.7210 0.024 0.000 0.036 0.940
#&gt; GSM11262     1  0.0000     0.8715 1.000 0.000 0.000 0.000
#&gt; GSM28724     4  0.3172     0.8222 0.160 0.000 0.000 0.840
#&gt; GSM28725     3  0.0000     0.7991 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000     0.7991 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000     0.7991 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.2032     0.7196 0.028 0.000 0.036 0.936
#&gt; GSM28734     4  0.2124     0.7169 0.028 0.000 0.040 0.932
#&gt; GSM28747     1  0.3172     0.8643 0.840 0.000 0.000 0.160
#&gt; GSM11257     4  0.1302     0.7599 0.044 0.000 0.000 0.956
#&gt; GSM11252     4  0.3494     0.8174 0.172 0.000 0.004 0.824
#&gt; GSM11264     3  0.0000     0.7991 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.4304     0.6097 0.000 0.000 0.716 0.284
#&gt; GSM11258     4  0.4746     0.3652 0.368 0.000 0.000 0.632
#&gt; GSM28728     4  0.3172     0.8222 0.160 0.000 0.000 0.840
#&gt; GSM28746     4  0.4356     0.7003 0.292 0.000 0.000 0.708
#&gt; GSM28738     4  0.3172     0.8222 0.160 0.000 0.000 0.840
#&gt; GSM28741     4  0.4883     0.5806 0.016 0.288 0.000 0.696
#&gt; GSM28729     4  0.3172     0.8222 0.160 0.000 0.000 0.840
#&gt; GSM28742     4  0.3172     0.8222 0.160 0.000 0.000 0.840
#&gt; GSM11250     4  0.5000     0.0582 0.000 0.496 0.000 0.504
#&gt; GSM11245     4  0.3494     0.8182 0.172 0.000 0.004 0.824
#&gt; GSM11246     1  0.0707     0.8767 0.980 0.000 0.000 0.020
#&gt; GSM11261     4  0.6263     0.0656 0.016 0.028 0.436 0.520
#&gt; GSM11248     3  0.4817     0.4384 0.000 0.000 0.612 0.388
#&gt; GSM28732     4  0.4250     0.6929 0.276 0.000 0.000 0.724
#&gt; GSM11255     4  0.4228     0.7781 0.232 0.000 0.008 0.760
#&gt; GSM28731     1  0.3172     0.8643 0.840 0.000 0.000 0.160
#&gt; GSM28727     1  0.3172     0.8643 0.840 0.000 0.000 0.160
#&gt; GSM11251     1  0.3172     0.8643 0.840 0.000 0.000 0.160
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.1341      0.748 0.000 0.000 0.000 0.056 0.944
#&gt; GSM28736     5  0.0510      0.759 0.000 0.000 0.000 0.016 0.984
#&gt; GSM28737     1  0.4021      0.775 0.780 0.000 0.000 0.052 0.168
#&gt; GSM11249     3  0.4297      0.718 0.000 0.000 0.764 0.164 0.072
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     5  0.5334      0.529 0.000 0.244 0.000 0.104 0.652
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000      0.807 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.807 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0000      0.917 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28740     1  0.0000      0.807 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     1  0.4014      0.751 0.728 0.000 0.000 0.016 0.256
#&gt; GSM28726     5  0.0703      0.764 0.000 0.000 0.000 0.024 0.976
#&gt; GSM28743     1  0.0000      0.807 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.3752      0.844 0.000 0.000 0.000 0.708 0.292
#&gt; GSM11262     1  0.0000      0.807 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28724     5  0.2875      0.750 0.052 0.000 0.008 0.056 0.884
#&gt; GSM28725     3  0.0000      0.917 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.917 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.917 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     4  0.3684      0.854 0.000 0.000 0.000 0.720 0.280
#&gt; GSM28734     4  0.2329      0.725 0.000 0.000 0.000 0.876 0.124
#&gt; GSM28747     1  0.3934      0.767 0.740 0.000 0.000 0.016 0.244
#&gt; GSM11257     5  0.0000      0.762 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11252     5  0.5527      0.632 0.156 0.000 0.004 0.176 0.664
#&gt; GSM11264     3  0.0000      0.917 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.0162      0.916 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11258     5  0.4905      0.616 0.256 0.000 0.016 0.036 0.692
#&gt; GSM28728     5  0.0613      0.763 0.004 0.000 0.004 0.008 0.984
#&gt; GSM28746     5  0.5940      0.581 0.200 0.000 0.004 0.184 0.612
#&gt; GSM28738     5  0.1341      0.748 0.000 0.000 0.000 0.056 0.944
#&gt; GSM28741     5  0.2482      0.748 0.000 0.024 0.000 0.084 0.892
#&gt; GSM28729     5  0.1341      0.748 0.000 0.000 0.000 0.056 0.944
#&gt; GSM28742     5  0.1270      0.750 0.000 0.000 0.000 0.052 0.948
#&gt; GSM11250     5  0.5045      0.594 0.000 0.196 0.000 0.108 0.696
#&gt; GSM11245     5  0.5523      0.644 0.124 0.000 0.008 0.200 0.668
#&gt; GSM11246     1  0.0162      0.806 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11261     5  0.4480      0.645 0.000 0.004 0.044 0.220 0.732
#&gt; GSM11248     3  0.4479      0.695 0.000 0.000 0.744 0.184 0.072
#&gt; GSM28732     5  0.3421      0.654 0.164 0.000 0.004 0.016 0.816
#&gt; GSM11255     5  0.6445      0.562 0.188 0.000 0.020 0.212 0.580
#&gt; GSM28731     1  0.3934      0.767 0.740 0.000 0.000 0.016 0.244
#&gt; GSM28727     1  0.3906      0.770 0.744 0.000 0.000 0.016 0.240
#&gt; GSM11251     1  0.3906      0.770 0.744 0.000 0.000 0.016 0.240
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.0260     0.6973 0.000 0.000 0.000 0.000 0.992 0.008
#&gt; GSM28736     5  0.1829     0.6763 0.000 0.000 0.000 0.024 0.920 0.056
#&gt; GSM28737     1  0.3028     0.8315 0.848 0.000 0.000 0.008 0.104 0.040
#&gt; GSM11249     3  0.4987     0.4953 0.000 0.000 0.584 0.016 0.048 0.352
#&gt; GSM28745     2  0.0000     0.9994 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9994 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     6  0.7242     0.2387 0.000 0.252 0.000 0.112 0.232 0.404
#&gt; GSM11266     2  0.0146     0.9956 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28730     2  0.0000     0.9994 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9994 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9994 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9994 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9994 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000     0.8418 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0260     0.8389 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM11243     3  0.0146     0.8485 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28740     1  0.0000     0.8418 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     1  0.4466     0.7968 0.736 0.000 0.000 0.060 0.176 0.028
#&gt; GSM28726     5  0.2908     0.6263 0.000 0.000 0.000 0.048 0.848 0.104
#&gt; GSM28743     1  0.0000     0.8418 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.3587     0.7964 0.000 0.000 0.000 0.772 0.188 0.040
#&gt; GSM11262     1  0.0000     0.8418 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28724     6  0.5612     0.1458 0.116 0.000 0.000 0.012 0.340 0.532
#&gt; GSM28725     3  0.0000     0.8494 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000     0.8494 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000     0.8494 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.3344     0.8268 0.000 0.000 0.000 0.804 0.152 0.044
#&gt; GSM28734     4  0.2980     0.6893 0.000 0.000 0.000 0.808 0.012 0.180
#&gt; GSM28747     1  0.4334     0.8139 0.752 0.000 0.000 0.060 0.160 0.028
#&gt; GSM11257     5  0.1531     0.6870 0.004 0.000 0.000 0.000 0.928 0.068
#&gt; GSM11252     6  0.6177     0.2160 0.092 0.000 0.000 0.056 0.404 0.448
#&gt; GSM11264     3  0.0000     0.8494 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.1267     0.8238 0.000 0.000 0.940 0.000 0.000 0.060
#&gt; GSM11258     5  0.6266    -0.1355 0.260 0.000 0.008 0.000 0.392 0.340
#&gt; GSM28728     5  0.5240     0.2073 0.076 0.000 0.000 0.016 0.588 0.320
#&gt; GSM28746     6  0.6306     0.2981 0.140 0.000 0.000 0.060 0.264 0.536
#&gt; GSM28738     5  0.0508     0.6962 0.000 0.000 0.000 0.004 0.984 0.012
#&gt; GSM28741     6  0.6079     0.0599 0.000 0.024 0.000 0.136 0.408 0.432
#&gt; GSM28729     5  0.0935     0.6966 0.000 0.000 0.000 0.004 0.964 0.032
#&gt; GSM28742     5  0.0508     0.6970 0.000 0.000 0.000 0.004 0.984 0.012
#&gt; GSM11250     6  0.7136     0.2396 0.000 0.196 0.000 0.116 0.252 0.436
#&gt; GSM11245     6  0.6263     0.2723 0.088 0.000 0.000 0.072 0.356 0.484
#&gt; GSM11246     1  0.0405     0.8424 0.988 0.000 0.000 0.000 0.004 0.008
#&gt; GSM11261     6  0.4912     0.3197 0.000 0.000 0.112 0.016 0.184 0.688
#&gt; GSM11248     3  0.5208     0.3608 0.000 0.000 0.500 0.020 0.048 0.432
#&gt; GSM28732     5  0.5701     0.0240 0.148 0.000 0.000 0.004 0.492 0.356
#&gt; GSM11255     6  0.5959     0.3557 0.144 0.000 0.008 0.056 0.164 0.628
#&gt; GSM28731     1  0.4334     0.8139 0.752 0.000 0.000 0.060 0.160 0.028
#&gt; GSM28727     1  0.4334     0.8139 0.752 0.000 0.000 0.060 0.160 0.028
#&gt; GSM11251     1  0.4334     0.8139 0.752 0.000 0.000 0.060 0.160 0.028
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:mclust 48     0.392 2
#> MAD:mclust 37     0.413 3
#> MAD:mclust 44     0.411 4
#> MAD:mclust 50     0.536 5
#> MAD:mclust 36     0.472 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.980       0.991         0.3906 0.607   0.607
#> 3 3 1.000           0.965       0.986         0.4890 0.731   0.588
#> 4 4 0.793           0.852       0.913         0.2486 0.837   0.620
#> 5 5 0.765           0.743       0.861         0.0860 0.929   0.743
#> 6 6 0.746           0.521       0.747         0.0449 0.949   0.768
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.0376      0.992 0.996 0.004
#&gt; GSM28736     2  0.3114      0.930 0.056 0.944
#&gt; GSM28737     1  0.0000      0.995 1.000 0.000
#&gt; GSM11249     1  0.0000      0.995 1.000 0.000
#&gt; GSM28745     2  0.0000      0.976 0.000 1.000
#&gt; GSM11244     2  0.0000      0.976 0.000 1.000
#&gt; GSM28748     2  0.0000      0.976 0.000 1.000
#&gt; GSM11266     2  0.0000      0.976 0.000 1.000
#&gt; GSM28730     2  0.0000      0.976 0.000 1.000
#&gt; GSM11253     2  0.0000      0.976 0.000 1.000
#&gt; GSM11254     2  0.0000      0.976 0.000 1.000
#&gt; GSM11260     2  0.0000      0.976 0.000 1.000
#&gt; GSM28733     2  0.0000      0.976 0.000 1.000
#&gt; GSM11265     1  0.0000      0.995 1.000 0.000
#&gt; GSM28739     1  0.0000      0.995 1.000 0.000
#&gt; GSM11243     1  0.0000      0.995 1.000 0.000
#&gt; GSM28740     1  0.0000      0.995 1.000 0.000
#&gt; GSM11259     1  0.0000      0.995 1.000 0.000
#&gt; GSM28726     2  0.7674      0.715 0.224 0.776
#&gt; GSM28743     1  0.0000      0.995 1.000 0.000
#&gt; GSM11256     1  0.0000      0.995 1.000 0.000
#&gt; GSM11262     1  0.0000      0.995 1.000 0.000
#&gt; GSM28724     1  0.0000      0.995 1.000 0.000
#&gt; GSM28725     1  0.0000      0.995 1.000 0.000
#&gt; GSM11263     1  0.0000      0.995 1.000 0.000
#&gt; GSM11267     1  0.0000      0.995 1.000 0.000
#&gt; GSM28744     1  0.0000      0.995 1.000 0.000
#&gt; GSM28734     1  0.0000      0.995 1.000 0.000
#&gt; GSM28747     1  0.0000      0.995 1.000 0.000
#&gt; GSM11257     1  0.0000      0.995 1.000 0.000
#&gt; GSM11252     1  0.0000      0.995 1.000 0.000
#&gt; GSM11264     1  0.0000      0.995 1.000 0.000
#&gt; GSM11247     1  0.0376      0.992 0.996 0.004
#&gt; GSM11258     1  0.0000      0.995 1.000 0.000
#&gt; GSM28728     1  0.0000      0.995 1.000 0.000
#&gt; GSM28746     1  0.0000      0.995 1.000 0.000
#&gt; GSM28738     1  0.0938      0.985 0.988 0.012
#&gt; GSM28741     2  0.0000      0.976 0.000 1.000
#&gt; GSM28729     1  0.0376      0.992 0.996 0.004
#&gt; GSM28742     1  0.2423      0.957 0.960 0.040
#&gt; GSM11250     2  0.0000      0.976 0.000 1.000
#&gt; GSM11245     1  0.0000      0.995 1.000 0.000
#&gt; GSM11246     1  0.0000      0.995 1.000 0.000
#&gt; GSM11261     1  0.5178      0.868 0.884 0.116
#&gt; GSM11248     1  0.0000      0.995 1.000 0.000
#&gt; GSM28732     1  0.0000      0.995 1.000 0.000
#&gt; GSM11255     1  0.0000      0.995 1.000 0.000
#&gt; GSM28731     1  0.0000      0.995 1.000 0.000
#&gt; GSM28727     1  0.0000      0.995 1.000 0.000
#&gt; GSM11251     1  0.0000      0.995 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28736     1  0.4931      0.696 0.768 0.232 0.000
#&gt; GSM28737     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11249     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11265     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28740     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11259     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28726     1  0.0237      0.973 0.996 0.004 0.000
#&gt; GSM28743     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11256     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11262     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28724     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28744     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28734     1  0.6215      0.262 0.572 0.000 0.428
#&gt; GSM28747     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11257     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11252     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11258     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28728     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28746     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28738     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28729     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28742     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11245     1  0.0424      0.970 0.992 0.000 0.008
#&gt; GSM11246     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11261     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11248     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28732     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11255     1  0.0592      0.966 0.988 0.000 0.012
#&gt; GSM28731     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000      0.976 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.976 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     4  0.3649      0.746 0.204 0.000 0.000 0.796
#&gt; GSM28736     4  0.4711      0.692 0.064 0.152 0.000 0.784
#&gt; GSM28737     1  0.0188      0.890 0.996 0.000 0.000 0.004
#&gt; GSM11249     3  0.0336      0.972 0.000 0.000 0.992 0.008
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.0336      0.888 0.992 0.000 0.000 0.008
#&gt; GSM28739     1  0.0469      0.888 0.988 0.000 0.000 0.012
#&gt; GSM11243     3  0.1867      0.931 0.000 0.000 0.928 0.072
#&gt; GSM28740     1  0.0336      0.889 0.992 0.000 0.000 0.008
#&gt; GSM11259     1  0.1716      0.865 0.936 0.000 0.000 0.064
#&gt; GSM28726     4  0.5055      0.610 0.368 0.008 0.000 0.624
#&gt; GSM28743     1  0.0469      0.889 0.988 0.000 0.000 0.012
#&gt; GSM11256     4  0.2401      0.760 0.092 0.000 0.004 0.904
#&gt; GSM11262     1  0.0921      0.885 0.972 0.000 0.000 0.028
#&gt; GSM28724     1  0.1576      0.876 0.948 0.000 0.004 0.048
#&gt; GSM28725     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0188      0.974 0.000 0.000 0.996 0.004
#&gt; GSM28744     4  0.3052      0.758 0.136 0.000 0.004 0.860
#&gt; GSM28734     4  0.3962      0.680 0.044 0.000 0.124 0.832
#&gt; GSM28747     1  0.1557      0.869 0.944 0.000 0.000 0.056
#&gt; GSM11257     4  0.3219      0.762 0.164 0.000 0.000 0.836
#&gt; GSM11252     1  0.4477      0.517 0.688 0.000 0.000 0.312
#&gt; GSM11264     3  0.0000      0.974 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.2647      0.889 0.000 0.000 0.880 0.120
#&gt; GSM11258     1  0.4585      0.458 0.668 0.000 0.000 0.332
#&gt; GSM28728     1  0.3626      0.707 0.812 0.000 0.004 0.184
#&gt; GSM28746     1  0.2469      0.824 0.892 0.000 0.000 0.108
#&gt; GSM28738     4  0.4761      0.549 0.372 0.000 0.000 0.628
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28729     4  0.4713      0.571 0.360 0.000 0.000 0.640
#&gt; GSM28742     4  0.4103      0.687 0.256 0.000 0.000 0.744
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11245     1  0.5577      0.431 0.636 0.000 0.036 0.328
#&gt; GSM11246     1  0.0000      0.890 1.000 0.000 0.000 0.000
#&gt; GSM11261     3  0.0336      0.972 0.000 0.000 0.992 0.008
#&gt; GSM11248     3  0.0188      0.974 0.000 0.000 0.996 0.004
#&gt; GSM28732     1  0.2081      0.850 0.916 0.000 0.000 0.084
#&gt; GSM11255     1  0.1284      0.886 0.964 0.000 0.012 0.024
#&gt; GSM28731     1  0.0817      0.886 0.976 0.000 0.000 0.024
#&gt; GSM28727     1  0.0707      0.888 0.980 0.000 0.000 0.020
#&gt; GSM11251     1  0.0469      0.890 0.988 0.000 0.000 0.012
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     4  0.4548     0.7740 0.120 0.000 0.000 0.752 0.128
#&gt; GSM28736     4  0.5568     0.7701 0.060 0.096 0.000 0.716 0.128
#&gt; GSM28737     1  0.1043     0.7620 0.960 0.000 0.000 0.000 0.040
#&gt; GSM11249     3  0.3005     0.8042 0.020 0.000 0.880 0.068 0.032
#&gt; GSM28745     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0404     0.7653 0.988 0.000 0.000 0.000 0.012
#&gt; GSM28739     1  0.0290     0.7652 0.992 0.000 0.000 0.000 0.008
#&gt; GSM11243     3  0.3534     0.7110 0.000 0.000 0.744 0.000 0.256
#&gt; GSM28740     1  0.0609     0.7616 0.980 0.000 0.000 0.020 0.000
#&gt; GSM11259     5  0.4437     0.2428 0.464 0.000 0.000 0.004 0.532
#&gt; GSM28726     5  0.6153     0.4689 0.208 0.000 0.000 0.232 0.560
#&gt; GSM28743     1  0.1638     0.7465 0.932 0.000 0.000 0.064 0.004
#&gt; GSM11256     4  0.1608     0.8611 0.000 0.000 0.000 0.928 0.072
#&gt; GSM11262     1  0.1608     0.7446 0.928 0.000 0.000 0.072 0.000
#&gt; GSM28724     1  0.4552    -0.0516 0.524 0.000 0.008 0.000 0.468
#&gt; GSM28725     3  0.0880     0.8704 0.000 0.000 0.968 0.000 0.032
#&gt; GSM11263     3  0.0404     0.8733 0.000 0.000 0.988 0.000 0.012
#&gt; GSM11267     3  0.0162     0.8711 0.000 0.000 0.996 0.000 0.004
#&gt; GSM28744     4  0.1121     0.8624 0.000 0.000 0.000 0.956 0.044
#&gt; GSM28734     4  0.0794     0.8210 0.028 0.000 0.000 0.972 0.000
#&gt; GSM28747     1  0.2824     0.7363 0.864 0.000 0.000 0.020 0.116
#&gt; GSM11257     4  0.3011     0.8375 0.016 0.000 0.000 0.844 0.140
#&gt; GSM11252     1  0.5350     0.5671 0.664 0.000 0.028 0.264 0.044
#&gt; GSM11264     3  0.0162     0.8728 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11247     3  0.4307     0.3030 0.000 0.000 0.504 0.000 0.496
#&gt; GSM11258     1  0.5111     0.4068 0.588 0.000 0.012 0.376 0.024
#&gt; GSM28728     5  0.3010     0.6664 0.172 0.000 0.004 0.000 0.824
#&gt; GSM28746     1  0.4495     0.6114 0.736 0.000 0.000 0.064 0.200
#&gt; GSM28738     5  0.2879     0.6466 0.032 0.000 0.000 0.100 0.868
#&gt; GSM28741     2  0.0510     0.9808 0.000 0.984 0.000 0.000 0.016
#&gt; GSM28729     5  0.3116     0.6754 0.064 0.000 0.000 0.076 0.860
#&gt; GSM28742     5  0.3165     0.6385 0.036 0.000 0.000 0.116 0.848
#&gt; GSM11250     2  0.0000     0.9981 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11245     1  0.6421     0.4820 0.592 0.000 0.104 0.260 0.044
#&gt; GSM11246     1  0.1043     0.7620 0.960 0.000 0.000 0.000 0.040
#&gt; GSM11261     3  0.1270     0.8648 0.000 0.000 0.948 0.000 0.052
#&gt; GSM11248     3  0.1911     0.8450 0.004 0.000 0.932 0.028 0.036
#&gt; GSM28732     5  0.4299     0.4220 0.388 0.000 0.000 0.004 0.608
#&gt; GSM11255     1  0.4187     0.6468 0.764 0.000 0.008 0.032 0.196
#&gt; GSM28731     1  0.4238     0.3050 0.628 0.000 0.000 0.004 0.368
#&gt; GSM28727     1  0.2358     0.7393 0.888 0.000 0.000 0.008 0.104
#&gt; GSM11251     1  0.1732     0.7468 0.920 0.000 0.000 0.000 0.080
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     4  0.6603     0.1908 0.136 0.000 0.000 0.440 0.068 0.356
#&gt; GSM28736     4  0.6413     0.4114 0.080 0.028 0.000 0.552 0.056 0.284
#&gt; GSM28737     1  0.0806     0.6333 0.972 0.000 0.000 0.000 0.020 0.008
#&gt; GSM11249     3  0.3790     0.6045 0.004 0.000 0.716 0.016 0.000 0.264
#&gt; GSM28745     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0632     0.6319 0.976 0.000 0.000 0.000 0.000 0.024
#&gt; GSM28739     1  0.1549     0.6188 0.936 0.000 0.000 0.000 0.020 0.044
#&gt; GSM11243     3  0.4700     0.5814 0.000 0.000 0.636 0.000 0.288 0.076
#&gt; GSM28740     1  0.0777     0.6320 0.972 0.000 0.000 0.004 0.000 0.024
#&gt; GSM11259     5  0.6058    -0.0244 0.324 0.000 0.000 0.000 0.404 0.272
#&gt; GSM28726     6  0.6656    -0.2320 0.164 0.004 0.000 0.048 0.344 0.440
#&gt; GSM28743     1  0.1265     0.6238 0.948 0.000 0.000 0.008 0.000 0.044
#&gt; GSM11256     4  0.0260     0.7061 0.000 0.000 0.000 0.992 0.008 0.000
#&gt; GSM11262     1  0.1196     0.6259 0.952 0.000 0.000 0.008 0.000 0.040
#&gt; GSM28724     5  0.6787     0.0172 0.308 0.000 0.028 0.004 0.344 0.316
#&gt; GSM28725     3  0.0935     0.7567 0.000 0.000 0.964 0.000 0.032 0.004
#&gt; GSM11263     3  0.0000     0.7601 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.1267     0.7517 0.000 0.000 0.940 0.000 0.000 0.060
#&gt; GSM28744     4  0.0000     0.7064 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28734     4  0.0909     0.6973 0.020 0.000 0.000 0.968 0.000 0.012
#&gt; GSM28747     1  0.4925     0.0477 0.512 0.000 0.000 0.000 0.064 0.424
#&gt; GSM11257     4  0.4756     0.5534 0.024 0.000 0.000 0.720 0.124 0.132
#&gt; GSM11252     6  0.5802     0.2303 0.348 0.000 0.068 0.052 0.000 0.532
#&gt; GSM11264     3  0.1007     0.7582 0.000 0.000 0.956 0.000 0.000 0.044
#&gt; GSM11247     3  0.5145     0.3834 0.000 0.000 0.484 0.000 0.432 0.084
#&gt; GSM11258     1  0.4591    -0.0321 0.500 0.000 0.000 0.464 0.000 0.036
#&gt; GSM28728     5  0.3789     0.3730 0.112 0.000 0.024 0.000 0.804 0.060
#&gt; GSM28746     1  0.6485     0.2582 0.560 0.000 0.000 0.112 0.152 0.176
#&gt; GSM28738     5  0.4382     0.4072 0.064 0.000 0.000 0.024 0.744 0.168
#&gt; GSM28741     2  0.3558     0.6614 0.000 0.760 0.000 0.000 0.028 0.212
#&gt; GSM28729     5  0.4449     0.3713 0.036 0.000 0.000 0.016 0.684 0.264
#&gt; GSM28742     5  0.4764     0.2729 0.020 0.000 0.000 0.020 0.540 0.420
#&gt; GSM11250     2  0.0000     0.9726 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11245     6  0.6718     0.2842 0.256 0.000 0.160 0.088 0.000 0.496
#&gt; GSM11246     1  0.0622     0.6341 0.980 0.000 0.000 0.000 0.008 0.012
#&gt; GSM11261     3  0.4418     0.6355 0.000 0.000 0.728 0.004 0.128 0.140
#&gt; GSM11248     3  0.3499     0.5792 0.000 0.000 0.680 0.000 0.000 0.320
#&gt; GSM28732     6  0.5802    -0.2004 0.180 0.000 0.000 0.000 0.400 0.420
#&gt; GSM11255     1  0.6369    -0.1706 0.436 0.000 0.040 0.000 0.148 0.376
#&gt; GSM28731     1  0.5654    -0.0992 0.444 0.000 0.000 0.000 0.404 0.152
#&gt; GSM28727     1  0.4859     0.1978 0.584 0.000 0.000 0.000 0.072 0.344
#&gt; GSM11251     1  0.3279     0.5021 0.796 0.000 0.000 0.000 0.028 0.176
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> MAD:NMF 50     0.394 2
#> MAD:NMF 49     0.368 3
#> MAD:NMF 48     0.430 4
#> MAD:NMF 42     0.422 5
#> MAD:NMF 31     0.474 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.2163 0.784   0.784
#> 3 3 1.000           0.999       1.000         1.3695 0.704   0.622
#> 4 4 0.881           0.859       0.943         0.2575 0.905   0.806
#> 5 5 0.915           0.893       0.939         0.0485 0.913   0.780
#> 6 6 0.848           0.821       0.916         0.0826 0.946   0.828
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2
#&gt; GSM28735     1       0          1  1  0
#&gt; GSM28736     1       0          1  1  0
#&gt; GSM28737     1       0          1  1  0
#&gt; GSM11249     1       0          1  1  0
#&gt; GSM28745     1       0          1  1  0
#&gt; GSM11244     1       0          1  1  0
#&gt; GSM28748     1       0          1  1  0
#&gt; GSM11266     1       0          1  1  0
#&gt; GSM28730     1       0          1  1  0
#&gt; GSM11253     1       0          1  1  0
#&gt; GSM11254     1       0          1  1  0
#&gt; GSM11260     1       0          1  1  0
#&gt; GSM28733     1       0          1  1  0
#&gt; GSM11265     1       0          1  1  0
#&gt; GSM28739     1       0          1  1  0
#&gt; GSM11243     2       0          1  0  1
#&gt; GSM28740     1       0          1  1  0
#&gt; GSM11259     1       0          1  1  0
#&gt; GSM28726     1       0          1  1  0
#&gt; GSM28743     1       0          1  1  0
#&gt; GSM11256     1       0          1  1  0
#&gt; GSM11262     1       0          1  1  0
#&gt; GSM28724     1       0          1  1  0
#&gt; GSM28725     2       0          1  0  1
#&gt; GSM11263     2       0          1  0  1
#&gt; GSM11267     2       0          1  0  1
#&gt; GSM28744     1       0          1  1  0
#&gt; GSM28734     1       0          1  1  0
#&gt; GSM28747     1       0          1  1  0
#&gt; GSM11257     1       0          1  1  0
#&gt; GSM11252     1       0          1  1  0
#&gt; GSM11264     2       0          1  0  1
#&gt; GSM11247     2       0          1  0  1
#&gt; GSM11258     1       0          1  1  0
#&gt; GSM28728     1       0          1  1  0
#&gt; GSM28746     1       0          1  1  0
#&gt; GSM28738     1       0          1  1  0
#&gt; GSM28741     1       0          1  1  0
#&gt; GSM28729     1       0          1  1  0
#&gt; GSM28742     1       0          1  1  0
#&gt; GSM11250     1       0          1  1  0
#&gt; GSM11245     1       0          1  1  0
#&gt; GSM11246     1       0          1  1  0
#&gt; GSM11261     1       0          1  1  0
#&gt; GSM11248     1       0          1  1  0
#&gt; GSM28732     1       0          1  1  0
#&gt; GSM11255     1       0          1  1  0
#&gt; GSM28731     1       0          1  1  0
#&gt; GSM28727     1       0          1  1  0
#&gt; GSM11251     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3
#&gt; GSM28735     1  0.0237      0.996 0.996 0.004  0
#&gt; GSM28736     1  0.0237      0.996 0.996 0.004  0
#&gt; GSM28737     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11249     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM11265     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28739     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000  1
#&gt; GSM28740     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11259     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28726     1  0.0237      0.996 0.996 0.004  0
#&gt; GSM28743     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11256     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11262     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28724     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000  1
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000  1
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000  1
#&gt; GSM28744     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28734     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28747     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11257     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11252     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000  1
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000  1
#&gt; GSM11258     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28728     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28746     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28738     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM28729     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28742     1  0.0237      0.996 0.996 0.004  0
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000  0
#&gt; GSM11245     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11246     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11261     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11248     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28732     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11255     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28731     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM28727     1  0.0000      0.999 1.000 0.000  0
#&gt; GSM11251     1  0.0000      0.999 1.000 0.000  0
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4
#&gt; GSM28735     1  0.4905      0.420 0.632 0.004  0 0.364
#&gt; GSM28736     1  0.4905      0.420 0.632 0.004  0 0.364
#&gt; GSM28737     1  0.0336      0.893 0.992 0.000  0 0.008
#&gt; GSM11249     4  0.0707      0.875 0.020 0.000  0 0.980
#&gt; GSM28745     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM11265     1  0.0000      0.894 1.000 0.000  0 0.000
#&gt; GSM28739     1  0.0000      0.894 1.000 0.000  0 0.000
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM28740     1  0.0000      0.894 1.000 0.000  0 0.000
#&gt; GSM11259     1  0.0336      0.893 0.992 0.000  0 0.008
#&gt; GSM28726     1  0.4905      0.420 0.632 0.004  0 0.364
#&gt; GSM28743     1  0.0336      0.893 0.992 0.000  0 0.008
#&gt; GSM11256     1  0.0707      0.888 0.980 0.000  0 0.020
#&gt; GSM11262     1  0.0336      0.893 0.992 0.000  0 0.008
#&gt; GSM28724     1  0.0000      0.894 1.000 0.000  0 0.000
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM28744     1  0.4817      0.319 0.612 0.000  0 0.388
#&gt; GSM28734     1  0.4925      0.224 0.572 0.000  0 0.428
#&gt; GSM28747     1  0.0000      0.894 1.000 0.000  0 0.000
#&gt; GSM11257     1  0.0707      0.888 0.980 0.000  0 0.020
#&gt; GSM11252     1  0.1474      0.863 0.948 0.000  0 0.052
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000  1 0.000
#&gt; GSM11258     1  0.0000      0.894 1.000 0.000  0 0.000
#&gt; GSM28728     1  0.0336      0.893 0.992 0.000  0 0.008
#&gt; GSM28746     1  0.0000      0.894 1.000 0.000  0 0.000
#&gt; GSM28738     1  0.0707      0.888 0.980 0.000  0 0.020
#&gt; GSM28741     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM28729     1  0.0469      0.892 0.988 0.000  0 0.012
#&gt; GSM28742     4  0.4088      0.652 0.232 0.004  0 0.764
#&gt; GSM11250     2  0.0000      1.000 0.000 1.000  0 0.000
#&gt; GSM11245     1  0.1474      0.863 0.948 0.000  0 0.052
#&gt; GSM11246     1  0.0000      0.894 1.000 0.000  0 0.000
#&gt; GSM11261     4  0.0707      0.875 0.020 0.000  0 0.980
#&gt; GSM11248     4  0.0707      0.875 0.020 0.000  0 0.980
#&gt; GSM28732     1  0.4730      0.425 0.636 0.000  0 0.364
#&gt; GSM11255     1  0.1474      0.863 0.948 0.000  0 0.052
#&gt; GSM28731     1  0.0469      0.892 0.988 0.000  0 0.012
#&gt; GSM28727     1  0.0000      0.894 1.000 0.000  0 0.000
#&gt; GSM11251     1  0.0000      0.894 1.000 0.000  0 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2 p3    p4    p5
#&gt; GSM28735     5  0.4249      0.823 0.432 0.000  0 0.000 0.568
#&gt; GSM28736     5  0.4249      0.823 0.432 0.000  0 0.000 0.568
#&gt; GSM28737     1  0.0703      0.915 0.976 0.000  0 0.000 0.024
#&gt; GSM11249     4  0.0162      0.990 0.004 0.000  0 0.996 0.000
#&gt; GSM28745     2  0.0000      0.999 0.000 1.000  0 0.000 0.000
#&gt; GSM11244     2  0.0000      0.999 0.000 1.000  0 0.000 0.000
#&gt; GSM28748     2  0.0000      0.999 0.000 1.000  0 0.000 0.000
#&gt; GSM11266     2  0.0000      0.999 0.000 1.000  0 0.000 0.000
#&gt; GSM28730     2  0.0000      0.999 0.000 1.000  0 0.000 0.000
#&gt; GSM11253     2  0.0000      0.999 0.000 1.000  0 0.000 0.000
#&gt; GSM11254     2  0.0000      0.999 0.000 1.000  0 0.000 0.000
#&gt; GSM11260     2  0.0000      0.999 0.000 1.000  0 0.000 0.000
#&gt; GSM28733     2  0.0000      0.999 0.000 1.000  0 0.000 0.000
#&gt; GSM11265     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
#&gt; GSM28739     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
#&gt; GSM11243     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM28740     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
#&gt; GSM11259     1  0.0703      0.915 0.976 0.000  0 0.000 0.024
#&gt; GSM28726     5  0.4249      0.823 0.432 0.000  0 0.000 0.568
#&gt; GSM28743     1  0.0703      0.915 0.976 0.000  0 0.000 0.024
#&gt; GSM11256     1  0.1041      0.909 0.964 0.000  0 0.004 0.032
#&gt; GSM11262     1  0.0703      0.915 0.976 0.000  0 0.000 0.024
#&gt; GSM28724     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
#&gt; GSM28725     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM11263     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM11267     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM28744     1  0.4436      0.225 0.596 0.000  0 0.396 0.008
#&gt; GSM28734     1  0.4256      0.164 0.564 0.000  0 0.436 0.000
#&gt; GSM28747     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
#&gt; GSM11257     1  0.1041      0.909 0.964 0.000  0 0.004 0.032
#&gt; GSM11252     1  0.1270      0.872 0.948 0.000  0 0.052 0.000
#&gt; GSM11264     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM11247     3  0.0000      1.000 0.000 0.000  1 0.000 0.000
#&gt; GSM11258     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
#&gt; GSM28728     1  0.0703      0.915 0.976 0.000  0 0.000 0.024
#&gt; GSM28746     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
#&gt; GSM28738     1  0.1041      0.909 0.964 0.000  0 0.004 0.032
#&gt; GSM28741     2  0.0162      0.996 0.000 0.996  0 0.000 0.004
#&gt; GSM28729     1  0.0865      0.914 0.972 0.000  0 0.004 0.024
#&gt; GSM28742     5  0.0880      0.112 0.032 0.000  0 0.000 0.968
#&gt; GSM11250     2  0.0162      0.996 0.000 0.996  0 0.000 0.004
#&gt; GSM11245     1  0.1270      0.872 0.948 0.000  0 0.052 0.000
#&gt; GSM11246     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
#&gt; GSM11261     4  0.0865      0.981 0.004 0.000  0 0.972 0.024
#&gt; GSM11248     4  0.0162      0.990 0.004 0.000  0 0.996 0.000
#&gt; GSM28732     5  0.4256      0.817 0.436 0.000  0 0.000 0.564
#&gt; GSM11255     1  0.1270      0.872 0.948 0.000  0 0.052 0.000
#&gt; GSM28731     1  0.0865      0.914 0.972 0.000  0 0.004 0.024
#&gt; GSM28727     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
#&gt; GSM11251     1  0.0000      0.917 1.000 0.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.3817      0.818 0.432 0.000 0.000 0.000 0.568 0.000
#&gt; GSM28736     5  0.3817      0.818 0.432 0.000 0.000 0.000 0.568 0.000
#&gt; GSM28737     1  0.1007      0.846 0.956 0.000 0.000 0.044 0.000 0.000
#&gt; GSM11249     6  0.0000      0.987 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28745     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.0260      0.995 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM28740     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11259     1  0.1007      0.846 0.956 0.000 0.000 0.044 0.000 0.000
#&gt; GSM28726     5  0.3817      0.818 0.432 0.000 0.000 0.000 0.568 0.000
#&gt; GSM28743     1  0.0632      0.856 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; GSM11256     4  0.3868     -0.142 0.496 0.000 0.000 0.504 0.000 0.000
#&gt; GSM11262     1  0.0632      0.856 0.976 0.000 0.000 0.024 0.000 0.000
#&gt; GSM28724     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28725     3  0.0000      0.997 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.997 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.997 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     1  0.4886      0.189 0.540 0.000 0.000 0.064 0.000 0.396
#&gt; GSM28734     1  0.3823      0.240 0.564 0.000 0.000 0.000 0.000 0.436
#&gt; GSM28747     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11257     4  0.0146      0.491 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM11252     1  0.1141      0.837 0.948 0.000 0.000 0.000 0.000 0.052
#&gt; GSM11264     3  0.0000      0.997 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0260      0.995 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11258     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28728     1  0.3101      0.631 0.756 0.000 0.000 0.244 0.000 0.000
#&gt; GSM28746     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28738     4  0.0146      0.491 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM28741     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28729     1  0.3101      0.630 0.756 0.000 0.000 0.244 0.000 0.000
#&gt; GSM28742     5  0.0260      0.140 0.008 0.000 0.000 0.000 0.992 0.000
#&gt; GSM11250     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11245     1  0.1141      0.837 0.948 0.000 0.000 0.000 0.000 0.052
#&gt; GSM11246     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11261     6  0.0777      0.974 0.000 0.000 0.000 0.004 0.024 0.972
#&gt; GSM11248     6  0.0000      0.987 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28732     5  0.3823      0.812 0.436 0.000 0.000 0.000 0.564 0.000
#&gt; GSM11255     1  0.1141      0.837 0.948 0.000 0.000 0.000 0.000 0.052
#&gt; GSM28731     1  0.3050      0.642 0.764 0.000 0.000 0.236 0.000 0.000
#&gt; GSM28727     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11251     1  0.0000      0.863 1.000 0.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:hclust 50     0.394 2
#> ATC:hclust 50     0.370 3
#> ATC:hclust 44     0.339 4
#> ATC:hclust 47     0.326 5
#> ATC:hclust 44     0.401 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.486           0.772       0.854         0.3326 0.556   0.556
#> 3 3 1.000           0.992       0.991         0.5526 0.919   0.856
#> 4 4 0.645           0.543       0.742         0.2998 0.778   0.542
#> 5 5 0.632           0.774       0.803         0.1125 0.795   0.420
#> 6 6 0.726           0.801       0.870         0.0678 0.958   0.826
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1   0.000     0.9423 1.000 0.000
#&gt; GSM28736     1   0.000     0.9423 1.000 0.000
#&gt; GSM28737     1   0.000     0.9423 1.000 0.000
#&gt; GSM11249     1   0.983    -0.0178 0.576 0.424
#&gt; GSM28745     2   0.983     0.6380 0.424 0.576
#&gt; GSM11244     2   0.983     0.6380 0.424 0.576
#&gt; GSM28748     2   0.983     0.6380 0.424 0.576
#&gt; GSM11266     2   0.983     0.6380 0.424 0.576
#&gt; GSM28730     2   0.983     0.6380 0.424 0.576
#&gt; GSM11253     2   0.983     0.6380 0.424 0.576
#&gt; GSM11254     2   0.983     0.6380 0.424 0.576
#&gt; GSM11260     2   0.983     0.6380 0.424 0.576
#&gt; GSM28733     2   0.983     0.6380 0.424 0.576
#&gt; GSM11265     1   0.000     0.9423 1.000 0.000
#&gt; GSM28739     1   0.000     0.9423 1.000 0.000
#&gt; GSM11243     2   0.925     0.4511 0.340 0.660
#&gt; GSM28740     1   0.000     0.9423 1.000 0.000
#&gt; GSM11259     1   0.000     0.9423 1.000 0.000
#&gt; GSM28726     1   0.000     0.9423 1.000 0.000
#&gt; GSM28743     1   0.000     0.9423 1.000 0.000
#&gt; GSM11256     1   0.000     0.9423 1.000 0.000
#&gt; GSM11262     1   0.000     0.9423 1.000 0.000
#&gt; GSM28724     1   0.000     0.9423 1.000 0.000
#&gt; GSM28725     2   0.925     0.4511 0.340 0.660
#&gt; GSM11263     2   0.925     0.4511 0.340 0.660
#&gt; GSM11267     2   0.925     0.4511 0.340 0.660
#&gt; GSM28744     1   0.000     0.9423 1.000 0.000
#&gt; GSM28734     1   0.000     0.9423 1.000 0.000
#&gt; GSM28747     1   0.000     0.9423 1.000 0.000
#&gt; GSM11257     1   0.000     0.9423 1.000 0.000
#&gt; GSM11252     1   0.000     0.9423 1.000 0.000
#&gt; GSM11264     2   0.925     0.4511 0.340 0.660
#&gt; GSM11247     2   0.925     0.4511 0.340 0.660
#&gt; GSM11258     1   0.000     0.9423 1.000 0.000
#&gt; GSM28728     1   0.000     0.9423 1.000 0.000
#&gt; GSM28746     1   0.000     0.9423 1.000 0.000
#&gt; GSM28738     1   0.000     0.9423 1.000 0.000
#&gt; GSM28741     1   0.925    -0.0040 0.660 0.340
#&gt; GSM28729     1   0.000     0.9423 1.000 0.000
#&gt; GSM28742     1   0.327     0.8386 0.940 0.060
#&gt; GSM11250     2   0.983     0.6380 0.424 0.576
#&gt; GSM11245     1   0.000     0.9423 1.000 0.000
#&gt; GSM11246     1   0.000     0.9423 1.000 0.000
#&gt; GSM11261     1   0.760     0.4195 0.780 0.220
#&gt; GSM11248     1   0.000     0.9423 1.000 0.000
#&gt; GSM28732     1   0.000     0.9423 1.000 0.000
#&gt; GSM11255     1   0.000     0.9423 1.000 0.000
#&gt; GSM28731     1   0.000     0.9423 1.000 0.000
#&gt; GSM28727     1   0.000     0.9423 1.000 0.000
#&gt; GSM11251     1   0.000     0.9423 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0237      0.993 0.996 0.000 0.004
#&gt; GSM28736     1  0.0237      0.993 0.996 0.000 0.004
#&gt; GSM28737     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM11249     3  0.1163      0.961 0.028 0.000 0.972
#&gt; GSM28745     2  0.0237      0.999 0.004 0.996 0.000
#&gt; GSM11244     2  0.0237      0.999 0.004 0.996 0.000
#&gt; GSM28748     2  0.0475      0.996 0.004 0.992 0.004
#&gt; GSM11266     2  0.0237      0.999 0.004 0.996 0.000
#&gt; GSM28730     2  0.0237      0.999 0.004 0.996 0.000
#&gt; GSM11253     2  0.0237      0.999 0.004 0.996 0.000
#&gt; GSM11254     2  0.0237      0.999 0.004 0.996 0.000
#&gt; GSM11260     2  0.0237      0.999 0.004 0.996 0.000
#&gt; GSM28733     2  0.0237      0.999 0.004 0.996 0.000
#&gt; GSM11265     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28739     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM11243     3  0.1267      0.993 0.004 0.024 0.972
#&gt; GSM28740     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM11259     1  0.0237      0.993 0.996 0.000 0.004
#&gt; GSM28726     1  0.0237      0.993 0.996 0.000 0.004
#&gt; GSM28743     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM11256     1  0.1129      0.978 0.976 0.004 0.020
#&gt; GSM11262     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28724     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28725     3  0.1267      0.993 0.004 0.024 0.972
#&gt; GSM11263     3  0.1267      0.993 0.004 0.024 0.972
#&gt; GSM11267     3  0.1267      0.993 0.004 0.024 0.972
#&gt; GSM28744     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28734     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28747     1  0.0237      0.993 0.996 0.000 0.004
#&gt; GSM11257     1  0.1129      0.978 0.976 0.004 0.020
#&gt; GSM11252     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM11264     3  0.1267      0.993 0.004 0.024 0.972
#&gt; GSM11247     3  0.1267      0.993 0.004 0.024 0.972
#&gt; GSM11258     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28728     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28746     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28738     1  0.1129      0.978 0.976 0.004 0.020
#&gt; GSM28741     1  0.2384      0.935 0.936 0.056 0.008
#&gt; GSM28729     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28742     1  0.0237      0.993 0.996 0.000 0.004
#&gt; GSM11250     2  0.0661      0.993 0.004 0.988 0.008
#&gt; GSM11245     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM11246     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM11261     1  0.0892      0.978 0.980 0.020 0.000
#&gt; GSM11248     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28732     1  0.0237      0.993 0.996 0.000 0.004
#&gt; GSM11255     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28731     1  0.0000      0.994 1.000 0.000 0.000
#&gt; GSM28727     1  0.0237      0.993 0.996 0.000 0.004
#&gt; GSM11251     1  0.0237      0.993 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.4431     0.3686 0.696 0.000 0.000 0.304
#&gt; GSM28736     1  0.4431     0.3686 0.696 0.000 0.000 0.304
#&gt; GSM28737     1  0.4981     0.6396 0.536 0.000 0.000 0.464
#&gt; GSM11249     3  0.5678     0.4534 0.024 0.000 0.524 0.452
#&gt; GSM28745     2  0.0000     0.9955 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9955 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0707     0.9838 0.020 0.980 0.000 0.000
#&gt; GSM11266     2  0.0000     0.9955 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9955 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9955 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9955 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9955 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9955 0.000 1.000 0.000 0.000
#&gt; GSM11265     4  0.3569     0.3177 0.196 0.000 0.000 0.804
#&gt; GSM28739     4  0.3569     0.3177 0.196 0.000 0.000 0.804
#&gt; GSM11243     3  0.1042     0.9144 0.020 0.008 0.972 0.000
#&gt; GSM28740     1  0.4981     0.6269 0.536 0.000 0.000 0.464
#&gt; GSM11259     1  0.4981     0.6386 0.536 0.000 0.000 0.464
#&gt; GSM28726     1  0.4431     0.3686 0.696 0.000 0.000 0.304
#&gt; GSM28743     1  0.4994     0.6228 0.520 0.000 0.000 0.480
#&gt; GSM11256     1  0.4456     0.5119 0.716 0.000 0.004 0.280
#&gt; GSM11262     1  0.4994     0.6228 0.520 0.000 0.000 0.480
#&gt; GSM28724     4  0.4961    -0.5118 0.448 0.000 0.000 0.552
#&gt; GSM28725     3  0.0336     0.9183 0.000 0.008 0.992 0.000
#&gt; GSM11263     3  0.0336     0.9183 0.000 0.008 0.992 0.000
#&gt; GSM11267     3  0.0336     0.9183 0.000 0.008 0.992 0.000
#&gt; GSM28744     4  0.2011     0.4797 0.080 0.000 0.000 0.920
#&gt; GSM28734     4  0.0817     0.5108 0.024 0.000 0.000 0.976
#&gt; GSM28747     4  0.4972    -0.5190 0.456 0.000 0.000 0.544
#&gt; GSM11257     1  0.4567     0.5105 0.716 0.000 0.008 0.276
#&gt; GSM11252     4  0.0188     0.5164 0.004 0.000 0.000 0.996
#&gt; GSM11264     3  0.0336     0.9183 0.000 0.008 0.992 0.000
#&gt; GSM11247     3  0.1042     0.9144 0.020 0.008 0.972 0.000
#&gt; GSM11258     4  0.4713    -0.0794 0.360 0.000 0.000 0.640
#&gt; GSM28728     1  0.4972     0.6303 0.544 0.000 0.000 0.456
#&gt; GSM28746     4  0.4277     0.0923 0.280 0.000 0.000 0.720
#&gt; GSM28738     1  0.4567     0.5105 0.716 0.000 0.008 0.276
#&gt; GSM28741     1  0.4663     0.3393 0.716 0.012 0.000 0.272
#&gt; GSM28729     1  0.4967     0.6298 0.548 0.000 0.000 0.452
#&gt; GSM28742     4  0.4134     0.3604 0.260 0.000 0.000 0.740
#&gt; GSM11250     2  0.0921     0.9772 0.028 0.972 0.000 0.000
#&gt; GSM11245     4  0.0188     0.5164 0.004 0.000 0.000 0.996
#&gt; GSM11246     4  0.4961    -0.5118 0.448 0.000 0.000 0.552
#&gt; GSM11261     4  0.4228     0.3651 0.232 0.008 0.000 0.760
#&gt; GSM11248     4  0.1118     0.5047 0.036 0.000 0.000 0.964
#&gt; GSM28732     4  0.4907     0.2450 0.420 0.000 0.000 0.580
#&gt; GSM11255     4  0.0000     0.5160 0.000 0.000 0.000 1.000
#&gt; GSM28731     1  0.4941     0.6382 0.564 0.000 0.000 0.436
#&gt; GSM28727     4  0.4981    -0.5364 0.464 0.000 0.000 0.536
#&gt; GSM11251     1  0.4981     0.6386 0.536 0.000 0.000 0.464
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2   p3    p4    p5
#&gt; GSM28735     5  0.4356      0.843 0.340 0.000 0.00 0.012 0.648
#&gt; GSM28736     5  0.4367      0.825 0.372 0.000 0.00 0.008 0.620
#&gt; GSM28737     1  0.0324      0.725 0.992 0.000 0.00 0.004 0.004
#&gt; GSM11249     4  0.3318      0.576 0.000 0.000 0.18 0.808 0.012
#&gt; GSM28745     2  0.0162      0.978 0.000 0.996 0.00 0.004 0.000
#&gt; GSM11244     2  0.0000      0.979 0.000 1.000 0.00 0.000 0.000
#&gt; GSM28748     2  0.2569      0.917 0.000 0.892 0.00 0.040 0.068
#&gt; GSM11266     2  0.0000      0.979 0.000 1.000 0.00 0.000 0.000
#&gt; GSM28730     2  0.0162      0.978 0.000 0.996 0.00 0.004 0.000
#&gt; GSM11253     2  0.0000      0.979 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11254     2  0.0000      0.979 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11260     2  0.0000      0.979 0.000 1.000 0.00 0.000 0.000
#&gt; GSM28733     2  0.0000      0.979 0.000 1.000 0.00 0.000 0.000
#&gt; GSM11265     1  0.5102      0.549 0.696 0.000 0.00 0.176 0.128
#&gt; GSM28739     1  0.5064      0.528 0.680 0.000 0.00 0.232 0.088
#&gt; GSM11243     3  0.1661      0.965 0.000 0.000 0.94 0.024 0.036
#&gt; GSM28740     1  0.0162      0.728 0.996 0.000 0.00 0.004 0.000
#&gt; GSM11259     1  0.1638      0.716 0.932 0.000 0.00 0.004 0.064
#&gt; GSM28726     5  0.4283      0.845 0.348 0.000 0.00 0.008 0.644
#&gt; GSM28743     1  0.0566      0.730 0.984 0.000 0.00 0.004 0.012
#&gt; GSM11256     1  0.5238      0.442 0.652 0.000 0.00 0.088 0.260
#&gt; GSM11262     1  0.0324      0.727 0.992 0.000 0.00 0.004 0.004
#&gt; GSM28724     1  0.3409      0.682 0.836 0.000 0.00 0.052 0.112
#&gt; GSM28725     3  0.0000      0.983 0.000 0.000 1.00 0.000 0.000
#&gt; GSM11263     3  0.0000      0.983 0.000 0.000 1.00 0.000 0.000
#&gt; GSM11267     3  0.0000      0.983 0.000 0.000 1.00 0.000 0.000
#&gt; GSM28744     4  0.5195      0.745 0.216 0.000 0.00 0.676 0.108
#&gt; GSM28734     4  0.3074      0.821 0.196 0.000 0.00 0.804 0.000
#&gt; GSM28747     1  0.3318      0.617 0.800 0.000 0.00 0.008 0.192
#&gt; GSM11257     1  0.5758      0.376 0.600 0.000 0.00 0.132 0.268
#&gt; GSM11252     4  0.4847      0.793 0.240 0.000 0.00 0.692 0.068
#&gt; GSM11264     3  0.0000      0.983 0.000 0.000 1.00 0.000 0.000
#&gt; GSM11247     3  0.1661      0.965 0.000 0.000 0.94 0.024 0.036
#&gt; GSM11258     1  0.3769      0.635 0.788 0.000 0.00 0.180 0.032
#&gt; GSM28728     1  0.3991      0.651 0.780 0.000 0.00 0.048 0.172
#&gt; GSM28746     1  0.4335      0.622 0.760 0.000 0.00 0.168 0.072
#&gt; GSM28738     1  0.5758      0.376 0.600 0.000 0.00 0.132 0.268
#&gt; GSM28741     5  0.4949      0.782 0.296 0.004 0.00 0.044 0.656
#&gt; GSM28729     1  0.3882      0.651 0.788 0.000 0.00 0.044 0.168
#&gt; GSM28742     5  0.5555      0.568 0.140 0.000 0.00 0.220 0.640
#&gt; GSM11250     2  0.2554      0.916 0.000 0.892 0.00 0.036 0.072
#&gt; GSM11245     4  0.4847      0.793 0.240 0.000 0.00 0.692 0.068
#&gt; GSM11246     1  0.3413      0.676 0.832 0.000 0.00 0.044 0.124
#&gt; GSM11261     4  0.3565      0.631 0.024 0.000 0.00 0.800 0.176
#&gt; GSM11248     4  0.3074      0.821 0.196 0.000 0.00 0.804 0.000
#&gt; GSM28732     5  0.5002      0.830 0.312 0.000 0.00 0.052 0.636
#&gt; GSM11255     4  0.4465      0.818 0.204 0.000 0.00 0.736 0.060
#&gt; GSM28731     1  0.2139      0.689 0.916 0.000 0.00 0.032 0.052
#&gt; GSM28727     1  0.3246      0.623 0.808 0.000 0.00 0.008 0.184
#&gt; GSM11251     1  0.1043      0.721 0.960 0.000 0.00 0.000 0.040
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.2848    0.87723 0.176 0.000 0.000 0.000 0.816 0.008
#&gt; GSM28736     5  0.2913    0.87406 0.180 0.000 0.000 0.004 0.812 0.004
#&gt; GSM28737     1  0.1471    0.74985 0.932 0.000 0.000 0.064 0.004 0.000
#&gt; GSM11249     6  0.1832    0.78553 0.000 0.000 0.032 0.032 0.008 0.928
#&gt; GSM28745     2  0.0291    0.95072 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; GSM11244     2  0.0000    0.95307 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.4128    0.80178 0.000 0.764 0.000 0.164 0.044 0.028
#&gt; GSM11266     2  0.0000    0.95307 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0717    0.94392 0.000 0.976 0.000 0.016 0.000 0.008
#&gt; GSM11253     2  0.0000    0.95307 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000    0.95307 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000    0.95307 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000    0.95307 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3225    0.74428 0.828 0.000 0.000 0.000 0.080 0.092
#&gt; GSM28739     1  0.3439    0.72780 0.808 0.000 0.000 0.000 0.072 0.120
#&gt; GSM11243     3  0.2537    0.91802 0.000 0.000 0.872 0.096 0.032 0.000
#&gt; GSM28740     1  0.0937    0.76843 0.960 0.000 0.000 0.040 0.000 0.000
#&gt; GSM11259     1  0.1434    0.75909 0.940 0.000 0.000 0.048 0.012 0.000
#&gt; GSM28726     5  0.2320    0.87368 0.132 0.000 0.000 0.000 0.864 0.004
#&gt; GSM28743     1  0.0603    0.77815 0.980 0.000 0.000 0.016 0.000 0.004
#&gt; GSM11256     4  0.4470    0.94165 0.268 0.000 0.000 0.680 0.036 0.016
#&gt; GSM11262     1  0.1267    0.75581 0.940 0.000 0.000 0.060 0.000 0.000
#&gt; GSM28724     1  0.2106    0.78468 0.904 0.000 0.000 0.000 0.064 0.032
#&gt; GSM28725     3  0.0000    0.96000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000    0.96000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000    0.96000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     6  0.6320    0.48201 0.116 0.000 0.000 0.216 0.100 0.568
#&gt; GSM28734     6  0.0972    0.83096 0.028 0.000 0.000 0.000 0.008 0.964
#&gt; GSM28747     1  0.2112    0.77728 0.896 0.000 0.000 0.000 0.088 0.016
#&gt; GSM11257     4  0.3831    0.97131 0.268 0.000 0.000 0.712 0.012 0.008
#&gt; GSM11252     6  0.3608    0.78059 0.148 0.000 0.000 0.000 0.064 0.788
#&gt; GSM11264     3  0.0000    0.96000 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.2537    0.91802 0.000 0.000 0.872 0.096 0.032 0.000
#&gt; GSM11258     1  0.2587    0.74913 0.868 0.000 0.000 0.004 0.020 0.108
#&gt; GSM28728     1  0.6202   -0.00471 0.508 0.000 0.000 0.256 0.212 0.024
#&gt; GSM28746     1  0.3206    0.74453 0.828 0.000 0.000 0.000 0.068 0.104
#&gt; GSM28738     4  0.3831    0.97131 0.268 0.000 0.000 0.712 0.012 0.008
#&gt; GSM28741     5  0.4785    0.73038 0.120 0.000 0.000 0.148 0.712 0.020
#&gt; GSM28729     1  0.6217   -0.01617 0.504 0.000 0.000 0.260 0.212 0.024
#&gt; GSM28742     5  0.2333    0.80875 0.040 0.000 0.000 0.004 0.896 0.060
#&gt; GSM11250     2  0.3930    0.80916 0.000 0.780 0.000 0.148 0.056 0.016
#&gt; GSM11245     6  0.3608    0.78059 0.148 0.000 0.000 0.000 0.064 0.788
#&gt; GSM11246     1  0.2066    0.78357 0.904 0.000 0.000 0.000 0.072 0.024
#&gt; GSM11261     6  0.1606    0.79912 0.004 0.000 0.000 0.008 0.056 0.932
#&gt; GSM11248     6  0.0858    0.82834 0.028 0.000 0.000 0.004 0.000 0.968
#&gt; GSM28732     5  0.3102    0.86890 0.156 0.000 0.000 0.000 0.816 0.028
#&gt; GSM11255     6  0.2786    0.82035 0.084 0.000 0.000 0.000 0.056 0.860
#&gt; GSM28731     1  0.3834    0.47399 0.748 0.000 0.000 0.216 0.028 0.008
#&gt; GSM28727     1  0.1812    0.77998 0.912 0.000 0.000 0.000 0.080 0.008
#&gt; GSM11251     1  0.0935    0.77155 0.964 0.000 0.000 0.032 0.004 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:kmeans 41     0.383 2
#> ATC:kmeans 50     0.370 3
#> ATC:kmeans 33     0.316 4
#> ATC:kmeans 47     0.404 5
#> ATC:kmeans 46     0.474 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.979       0.986         0.4743 0.519   0.519
#> 3 3 1.000           0.981       0.992         0.3176 0.784   0.610
#> 4 4 0.738           0.591       0.814         0.1768 0.932   0.820
#> 5 5 0.794           0.529       0.744         0.0757 0.864   0.589
#> 6 6 0.878           0.849       0.915         0.0483 0.924   0.674
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1   0.000      0.996 1.000 0.000
#&gt; GSM28736     1   0.184      0.969 0.972 0.028
#&gt; GSM28737     1   0.000      0.996 1.000 0.000
#&gt; GSM11249     1   0.000      0.996 1.000 0.000
#&gt; GSM28745     2   0.000      0.967 0.000 1.000
#&gt; GSM11244     2   0.000      0.967 0.000 1.000
#&gt; GSM28748     2   0.000      0.967 0.000 1.000
#&gt; GSM11266     2   0.000      0.967 0.000 1.000
#&gt; GSM28730     2   0.000      0.967 0.000 1.000
#&gt; GSM11253     2   0.000      0.967 0.000 1.000
#&gt; GSM11254     2   0.000      0.967 0.000 1.000
#&gt; GSM11260     2   0.000      0.967 0.000 1.000
#&gt; GSM28733     2   0.000      0.967 0.000 1.000
#&gt; GSM11265     1   0.000      0.996 1.000 0.000
#&gt; GSM28739     1   0.000      0.996 1.000 0.000
#&gt; GSM11243     2   0.416      0.941 0.084 0.916
#&gt; GSM28740     1   0.000      0.996 1.000 0.000
#&gt; GSM11259     1   0.000      0.996 1.000 0.000
#&gt; GSM28726     1   0.443      0.902 0.908 0.092
#&gt; GSM28743     1   0.000      0.996 1.000 0.000
#&gt; GSM11256     1   0.000      0.996 1.000 0.000
#&gt; GSM11262     1   0.000      0.996 1.000 0.000
#&gt; GSM28724     1   0.000      0.996 1.000 0.000
#&gt; GSM28725     2   0.416      0.941 0.084 0.916
#&gt; GSM11263     2   0.416      0.941 0.084 0.916
#&gt; GSM11267     2   0.416      0.941 0.084 0.916
#&gt; GSM28744     1   0.000      0.996 1.000 0.000
#&gt; GSM28734     1   0.000      0.996 1.000 0.000
#&gt; GSM28747     1   0.000      0.996 1.000 0.000
#&gt; GSM11257     1   0.000      0.996 1.000 0.000
#&gt; GSM11252     1   0.000      0.996 1.000 0.000
#&gt; GSM11264     2   0.416      0.941 0.084 0.916
#&gt; GSM11247     2   0.416      0.941 0.084 0.916
#&gt; GSM11258     1   0.000      0.996 1.000 0.000
#&gt; GSM28728     1   0.000      0.996 1.000 0.000
#&gt; GSM28746     1   0.000      0.996 1.000 0.000
#&gt; GSM28738     1   0.000      0.996 1.000 0.000
#&gt; GSM28741     2   0.000      0.967 0.000 1.000
#&gt; GSM28729     1   0.000      0.996 1.000 0.000
#&gt; GSM28742     2   0.278      0.956 0.048 0.952
#&gt; GSM11250     2   0.000      0.967 0.000 1.000
#&gt; GSM11245     1   0.000      0.996 1.000 0.000
#&gt; GSM11246     1   0.000      0.996 1.000 0.000
#&gt; GSM11261     2   0.278      0.956 0.048 0.952
#&gt; GSM11248     1   0.000      0.996 1.000 0.000
#&gt; GSM28732     1   0.000      0.996 1.000 0.000
#&gt; GSM11255     1   0.000      0.996 1.000 0.000
#&gt; GSM28731     1   0.000      0.996 1.000 0.000
#&gt; GSM28727     1   0.000      0.996 1.000 0.000
#&gt; GSM11251     1   0.000      0.996 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28736     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28737     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11249     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM28745     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM11244     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM28748     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM11266     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM28730     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM11253     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM11254     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM11260     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM28733     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM11265     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28739     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11243     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM28740     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11259     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28726     2   0.529      0.632 0.268 0.732 0.000
#&gt; GSM28743     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11256     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11262     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28724     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28725     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM11263     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM11267     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM28744     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28734     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM28747     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11257     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11252     1   0.164      0.956 0.956 0.000 0.044
#&gt; GSM11264     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM11247     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM11258     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28728     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28746     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28738     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28741     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM28729     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28742     3   0.116      0.970 0.000 0.028 0.972
#&gt; GSM11250     2   0.000      0.969 0.000 1.000 0.000
#&gt; GSM11245     1   0.164      0.956 0.956 0.000 0.044
#&gt; GSM11246     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11261     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM11248     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM28732     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11255     3   0.000      0.997 0.000 0.000 1.000
#&gt; GSM28731     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28727     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11251     1   0.000      0.996 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     4  0.4331    0.71220 0.288 0.000 0.000 0.712
#&gt; GSM28736     4  0.4331    0.71220 0.288 0.000 0.000 0.712
#&gt; GSM28737     1  0.3123    0.45285 0.844 0.000 0.000 0.156
#&gt; GSM11249     3  0.0188    0.86619 0.000 0.000 0.996 0.004
#&gt; GSM28745     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.4304    0.44529 0.716 0.000 0.000 0.284
#&gt; GSM28739     1  0.4304    0.44529 0.716 0.000 0.000 0.284
#&gt; GSM11243     3  0.0000    0.86765 0.000 0.000 1.000 0.000
#&gt; GSM28740     1  0.2149    0.51061 0.912 0.000 0.000 0.088
#&gt; GSM11259     1  0.3907    0.35032 0.768 0.000 0.000 0.232
#&gt; GSM28726     4  0.5990    0.66746 0.284 0.072 0.000 0.644
#&gt; GSM28743     1  0.0336    0.54025 0.992 0.000 0.000 0.008
#&gt; GSM11256     1  0.4888   -0.04750 0.588 0.000 0.000 0.412
#&gt; GSM11262     1  0.2216    0.50816 0.908 0.000 0.000 0.092
#&gt; GSM28724     1  0.0469    0.54162 0.988 0.000 0.000 0.012
#&gt; GSM28725     3  0.0000    0.86765 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000    0.86765 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000    0.86765 0.000 0.000 1.000 0.000
#&gt; GSM28744     4  0.4222    0.02909 0.272 0.000 0.000 0.728
#&gt; GSM28734     3  0.6750    0.55273 0.128 0.000 0.584 0.288
#&gt; GSM28747     1  0.0336    0.54157 0.992 0.000 0.000 0.008
#&gt; GSM11257     1  0.4790    0.05263 0.620 0.000 0.000 0.380
#&gt; GSM11252     1  0.6307    0.36346 0.620 0.000 0.092 0.288
#&gt; GSM11264     3  0.0000    0.86765 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.0000    0.86765 0.000 0.000 1.000 0.000
#&gt; GSM11258     1  0.4304    0.44529 0.716 0.000 0.000 0.284
#&gt; GSM28728     1  0.4961   -0.16530 0.552 0.000 0.000 0.448
#&gt; GSM28746     1  0.4304    0.44529 0.716 0.000 0.000 0.284
#&gt; GSM28738     1  0.4843    0.00515 0.604 0.000 0.000 0.396
#&gt; GSM28741     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM28729     1  0.4961   -0.16530 0.552 0.000 0.000 0.448
#&gt; GSM28742     3  0.4800    0.51818 0.000 0.004 0.656 0.340
#&gt; GSM11250     2  0.0000    1.00000 0.000 1.000 0.000 0.000
#&gt; GSM11245     1  0.6307    0.36346 0.620 0.000 0.092 0.288
#&gt; GSM11246     1  0.1716    0.52714 0.936 0.000 0.000 0.064
#&gt; GSM11261     3  0.0000    0.86765 0.000 0.000 1.000 0.000
#&gt; GSM11248     3  0.3764    0.73593 0.000 0.000 0.784 0.216
#&gt; GSM28732     1  0.4985   -0.14567 0.532 0.000 0.000 0.468
#&gt; GSM11255     3  0.6835    0.54268 0.136 0.000 0.576 0.288
#&gt; GSM28731     1  0.4746    0.08338 0.632 0.000 0.000 0.368
#&gt; GSM28727     1  0.0000    0.54133 1.000 0.000 0.000 0.000
#&gt; GSM11251     1  0.2814    0.47625 0.868 0.000 0.000 0.132
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.4774     0.4489 0.028 0.000 0.360 0.000 0.612
#&gt; GSM28736     5  0.4774     0.4541 0.028 0.000 0.360 0.000 0.612
#&gt; GSM28737     1  0.0609     0.6451 0.980 0.000 0.000 0.000 0.020
#&gt; GSM11249     4  0.4291    -0.5667 0.000 0.000 0.464 0.536 0.000
#&gt; GSM28745     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.4403     0.2924 0.560 0.000 0.000 0.436 0.004
#&gt; GSM28739     1  0.4268     0.2850 0.556 0.000 0.000 0.444 0.000
#&gt; GSM11243     3  0.4060     0.8110 0.000 0.000 0.640 0.360 0.000
#&gt; GSM28740     1  0.0404     0.6495 0.988 0.000 0.000 0.000 0.012
#&gt; GSM11259     1  0.2068     0.5872 0.904 0.000 0.004 0.000 0.092
#&gt; GSM28726     5  0.5616     0.4374 0.040 0.024 0.360 0.000 0.576
#&gt; GSM28743     1  0.1478     0.6684 0.936 0.000 0.000 0.064 0.000
#&gt; GSM11256     5  0.4557     0.0349 0.476 0.000 0.000 0.008 0.516
#&gt; GSM11262     1  0.0693     0.6569 0.980 0.000 0.000 0.012 0.008
#&gt; GSM28724     1  0.1671     0.6689 0.924 0.000 0.000 0.076 0.000
#&gt; GSM28725     3  0.4060     0.8110 0.000 0.000 0.640 0.360 0.000
#&gt; GSM11263     3  0.4060     0.8110 0.000 0.000 0.640 0.360 0.000
#&gt; GSM11267     3  0.4060     0.8110 0.000 0.000 0.640 0.360 0.000
#&gt; GSM28744     5  0.5992     0.0446 0.112 0.000 0.000 0.416 0.472
#&gt; GSM28734     4  0.0324     0.4821 0.004 0.000 0.004 0.992 0.000
#&gt; GSM28747     1  0.3099     0.6510 0.848 0.000 0.000 0.124 0.028
#&gt; GSM11257     1  0.4304    -0.1161 0.516 0.000 0.000 0.000 0.484
#&gt; GSM11252     4  0.4232     0.2212 0.312 0.000 0.000 0.676 0.012
#&gt; GSM11264     3  0.4060     0.8110 0.000 0.000 0.640 0.360 0.000
#&gt; GSM11247     3  0.4060     0.8110 0.000 0.000 0.640 0.360 0.000
#&gt; GSM11258     1  0.4262     0.2927 0.560 0.000 0.000 0.440 0.000
#&gt; GSM28728     5  0.4622     0.1079 0.440 0.000 0.000 0.012 0.548
#&gt; GSM28746     1  0.4410     0.2905 0.556 0.000 0.000 0.440 0.004
#&gt; GSM28738     1  0.4307    -0.1436 0.504 0.000 0.000 0.000 0.496
#&gt; GSM28741     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28729     5  0.4617     0.1139 0.436 0.000 0.000 0.012 0.552
#&gt; GSM28742     3  0.4294    -0.3896 0.000 0.000 0.532 0.000 0.468
#&gt; GSM11250     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11245     4  0.4127     0.2266 0.312 0.000 0.000 0.680 0.008
#&gt; GSM11246     1  0.3011     0.6482 0.844 0.000 0.000 0.140 0.016
#&gt; GSM11261     3  0.4060     0.8110 0.000 0.000 0.640 0.360 0.000
#&gt; GSM11248     4  0.3636    -0.0639 0.000 0.000 0.272 0.728 0.000
#&gt; GSM28732     5  0.5912     0.4240 0.088 0.000 0.360 0.008 0.544
#&gt; GSM11255     4  0.0290     0.4879 0.008 0.000 0.000 0.992 0.000
#&gt; GSM28731     1  0.4297    -0.0934 0.528 0.000 0.000 0.000 0.472
#&gt; GSM28727     1  0.2511     0.6619 0.892 0.000 0.000 0.080 0.028
#&gt; GSM11251     1  0.0510     0.6516 0.984 0.000 0.000 0.000 0.016
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.1262      0.938 0.016  0 0.000 0.020 0.956 0.008
#&gt; GSM28736     5  0.2425      0.892 0.012  0 0.000 0.100 0.880 0.008
#&gt; GSM28737     1  0.2378      0.766 0.848  0 0.000 0.152 0.000 0.000
#&gt; GSM11249     3  0.3695      0.330 0.000  0 0.624 0.000 0.000 0.376
#&gt; GSM28745     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.3665      0.611 0.696  0 0.000 0.004 0.004 0.296
#&gt; GSM28739     1  0.3756      0.545 0.644  0 0.000 0.004 0.000 0.352
#&gt; GSM11243     3  0.0000      0.939 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM28740     1  0.2191      0.793 0.876  0 0.000 0.120 0.000 0.004
#&gt; GSM11259     1  0.3023      0.693 0.784  0 0.000 0.212 0.004 0.000
#&gt; GSM28726     5  0.0622      0.936 0.000  0 0.000 0.012 0.980 0.008
#&gt; GSM28743     1  0.1141      0.817 0.948  0 0.000 0.052 0.000 0.000
#&gt; GSM11256     4  0.0713      0.878 0.028  0 0.000 0.972 0.000 0.000
#&gt; GSM11262     1  0.1663      0.808 0.912  0 0.000 0.088 0.000 0.000
#&gt; GSM28724     1  0.1461      0.820 0.940  0 0.000 0.044 0.000 0.016
#&gt; GSM28725     3  0.0000      0.939 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.939 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.939 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.3468      0.548 0.000  0 0.000 0.712 0.004 0.284
#&gt; GSM28734     6  0.1138      0.884 0.012  0 0.024 0.004 0.000 0.960
#&gt; GSM28747     1  0.1251      0.815 0.956  0 0.000 0.012 0.008 0.024
#&gt; GSM11257     4  0.1970      0.868 0.092  0 0.000 0.900 0.000 0.008
#&gt; GSM11252     6  0.0777      0.887 0.024  0 0.004 0.000 0.000 0.972
#&gt; GSM11264     3  0.0000      0.939 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.0000      0.939 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM11258     1  0.4264      0.578 0.636  0 0.000 0.032 0.000 0.332
#&gt; GSM28728     4  0.1421      0.871 0.028  0 0.000 0.944 0.028 0.000
#&gt; GSM28746     1  0.3861      0.543 0.640  0 0.000 0.008 0.000 0.352
#&gt; GSM28738     4  0.1812      0.874 0.080  0 0.000 0.912 0.000 0.008
#&gt; GSM28741     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM28729     4  0.1572      0.873 0.036  0 0.000 0.936 0.028 0.000
#&gt; GSM28742     5  0.1409      0.923 0.000  0 0.032 0.012 0.948 0.008
#&gt; GSM11250     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM11245     6  0.0777      0.887 0.024  0 0.004 0.000 0.000 0.972
#&gt; GSM11246     1  0.1194      0.812 0.956  0 0.000 0.008 0.004 0.032
#&gt; GSM11261     3  0.0146      0.936 0.000  0 0.996 0.000 0.000 0.004
#&gt; GSM11248     6  0.3601      0.471 0.000  0 0.312 0.004 0.000 0.684
#&gt; GSM28732     5  0.2002      0.924 0.040  0 0.000 0.012 0.920 0.028
#&gt; GSM11255     6  0.0914      0.889 0.016  0 0.016 0.000 0.000 0.968
#&gt; GSM28731     4  0.2527      0.800 0.168  0 0.000 0.832 0.000 0.000
#&gt; GSM28727     1  0.1092      0.814 0.960  0 0.000 0.020 0.020 0.000
#&gt; GSM11251     1  0.2020      0.802 0.896  0 0.000 0.096 0.008 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) k
#> ATC:skmeans 50     0.394 2
#> ATC:skmeans 50     0.370 3
#> ATC:skmeans 33     0.397 4
#> ATC:skmeans 28     0.400 5
#> ATC:skmeans 48     0.399 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.3272 0.673   0.673
#> 3 3 1.000           1.000       1.000         0.5084 0.833   0.753
#> 4 4 0.804           0.922       0.954         0.1605 0.948   0.897
#> 5 5 0.831           0.948       0.972         0.2292 0.843   0.655
#> 6 6 0.811           0.942       0.965         0.0123 0.993   0.978
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2
#&gt; GSM28735     1       0          1  1  0
#&gt; GSM28736     1       0          1  1  0
#&gt; GSM28737     1       0          1  1  0
#&gt; GSM11249     1       0          1  1  0
#&gt; GSM28745     2       0          1  0  1
#&gt; GSM11244     2       0          1  0  1
#&gt; GSM28748     2       0          1  0  1
#&gt; GSM11266     2       0          1  0  1
#&gt; GSM28730     2       0          1  0  1
#&gt; GSM11253     2       0          1  0  1
#&gt; GSM11254     2       0          1  0  1
#&gt; GSM11260     2       0          1  0  1
#&gt; GSM28733     2       0          1  0  1
#&gt; GSM11265     1       0          1  1  0
#&gt; GSM28739     1       0          1  1  0
#&gt; GSM11243     1       0          1  1  0
#&gt; GSM28740     1       0          1  1  0
#&gt; GSM11259     1       0          1  1  0
#&gt; GSM28726     1       0          1  1  0
#&gt; GSM28743     1       0          1  1  0
#&gt; GSM11256     1       0          1  1  0
#&gt; GSM11262     1       0          1  1  0
#&gt; GSM28724     1       0          1  1  0
#&gt; GSM28725     1       0          1  1  0
#&gt; GSM11263     1       0          1  1  0
#&gt; GSM11267     1       0          1  1  0
#&gt; GSM28744     1       0          1  1  0
#&gt; GSM28734     1       0          1  1  0
#&gt; GSM28747     1       0          1  1  0
#&gt; GSM11257     1       0          1  1  0
#&gt; GSM11252     1       0          1  1  0
#&gt; GSM11264     1       0          1  1  0
#&gt; GSM11247     1       0          1  1  0
#&gt; GSM11258     1       0          1  1  0
#&gt; GSM28728     1       0          1  1  0
#&gt; GSM28746     1       0          1  1  0
#&gt; GSM28738     1       0          1  1  0
#&gt; GSM28741     1       0          1  1  0
#&gt; GSM28729     1       0          1  1  0
#&gt; GSM28742     1       0          1  1  0
#&gt; GSM11250     2       0          1  0  1
#&gt; GSM11245     1       0          1  1  0
#&gt; GSM11246     1       0          1  1  0
#&gt; GSM11261     1       0          1  1  0
#&gt; GSM11248     1       0          1  1  0
#&gt; GSM28732     1       0          1  1  0
#&gt; GSM11255     1       0          1  1  0
#&gt; GSM28731     1       0          1  1  0
#&gt; GSM28727     1       0          1  1  0
#&gt; GSM11251     1       0          1  1  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette p1 p2 p3
#&gt; GSM28735     1       0          1  1  0  0
#&gt; GSM28736     1       0          1  1  0  0
#&gt; GSM28737     1       0          1  1  0  0
#&gt; GSM11249     1       0          1  1  0  0
#&gt; GSM28745     2       0          1  0  1  0
#&gt; GSM11244     2       0          1  0  1  0
#&gt; GSM28748     2       0          1  0  1  0
#&gt; GSM11266     2       0          1  0  1  0
#&gt; GSM28730     2       0          1  0  1  0
#&gt; GSM11253     2       0          1  0  1  0
#&gt; GSM11254     2       0          1  0  1  0
#&gt; GSM11260     2       0          1  0  1  0
#&gt; GSM28733     2       0          1  0  1  0
#&gt; GSM11265     1       0          1  1  0  0
#&gt; GSM28739     1       0          1  1  0  0
#&gt; GSM11243     3       0          1  0  0  1
#&gt; GSM28740     1       0          1  1  0  0
#&gt; GSM11259     1       0          1  1  0  0
#&gt; GSM28726     1       0          1  1  0  0
#&gt; GSM28743     1       0          1  1  0  0
#&gt; GSM11256     1       0          1  1  0  0
#&gt; GSM11262     1       0          1  1  0  0
#&gt; GSM28724     1       0          1  1  0  0
#&gt; GSM28725     3       0          1  0  0  1
#&gt; GSM11263     3       0          1  0  0  1
#&gt; GSM11267     3       0          1  0  0  1
#&gt; GSM28744     1       0          1  1  0  0
#&gt; GSM28734     1       0          1  1  0  0
#&gt; GSM28747     1       0          1  1  0  0
#&gt; GSM11257     1       0          1  1  0  0
#&gt; GSM11252     1       0          1  1  0  0
#&gt; GSM11264     3       0          1  0  0  1
#&gt; GSM11247     3       0          1  0  0  1
#&gt; GSM11258     1       0          1  1  0  0
#&gt; GSM28728     1       0          1  1  0  0
#&gt; GSM28746     1       0          1  1  0  0
#&gt; GSM28738     1       0          1  1  0  0
#&gt; GSM28741     1       0          1  1  0  0
#&gt; GSM28729     1       0          1  1  0  0
#&gt; GSM28742     1       0          1  1  0  0
#&gt; GSM11250     2       0          1  0  1  0
#&gt; GSM11245     1       0          1  1  0  0
#&gt; GSM11246     1       0          1  1  0  0
#&gt; GSM11261     1       0          1  1  0  0
#&gt; GSM11248     1       0          1  1  0  0
#&gt; GSM28732     1       0          1  1  0  0
#&gt; GSM11255     1       0          1  1  0  0
#&gt; GSM28731     1       0          1  1  0  0
#&gt; GSM28727     1       0          1  1  0  0
#&gt; GSM11251     1       0          1  1  0  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4
#&gt; GSM28735     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28736     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28737     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM11249     1  0.3907      0.745 0.768  0 0.000 0.232
#&gt; GSM28745     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11265     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28739     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM11243     3  0.0000      0.997 0.000  0 1.000 0.000
#&gt; GSM28740     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM11259     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28726     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28743     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM11256     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM11262     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28724     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28725     3  0.0000      0.997 0.000  0 1.000 0.000
#&gt; GSM11263     3  0.0000      0.997 0.000  0 1.000 0.000
#&gt; GSM11267     3  0.0000      0.997 0.000  0 1.000 0.000
#&gt; GSM28744     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28734     1  0.3907      0.745 0.768  0 0.000 0.232
#&gt; GSM28747     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM11257     4  0.3907      1.000 0.232  0 0.000 0.768
#&gt; GSM11252     1  0.3907      0.745 0.768  0 0.000 0.232
#&gt; GSM11264     3  0.0000      0.997 0.000  0 1.000 0.000
#&gt; GSM11247     3  0.0469      0.985 0.000  0 0.988 0.012
#&gt; GSM11258     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28728     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28746     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28738     4  0.3907      1.000 0.232  0 0.000 0.768
#&gt; GSM28741     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28729     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28742     1  0.3907      0.745 0.768  0 0.000 0.232
#&gt; GSM11250     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM11245     1  0.3837      0.752 0.776  0 0.000 0.224
#&gt; GSM11246     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM11261     1  0.3907      0.745 0.768  0 0.000 0.232
#&gt; GSM11248     1  0.3907      0.745 0.768  0 0.000 0.232
#&gt; GSM28732     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM11255     1  0.3907      0.745 0.768  0 0.000 0.232
#&gt; GSM28731     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM28727     1  0.0000      0.923 1.000  0 0.000 0.000
#&gt; GSM11251     1  0.0000      0.923 1.000  0 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2   p3    p4 p5
#&gt; GSM28735     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28736     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28737     1   0.202      0.914 0.900  0 0.00 0.100  0
#&gt; GSM11249     4   0.000      0.871 0.000  0 0.00 1.000  0
#&gt; GSM28745     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM11244     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM28748     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM11266     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM28730     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM11253     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM11254     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM11260     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM28733     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM11265     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28739     1   0.202      0.914 0.900  0 0.00 0.100  0
#&gt; GSM11243     3   0.000      0.940 0.000  0 1.00 0.000  0
#&gt; GSM28740     1   0.202      0.914 0.900  0 0.00 0.100  0
#&gt; GSM11259     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28726     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28743     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM11256     1   0.202      0.914 0.900  0 0.00 0.100  0
#&gt; GSM11262     1   0.202      0.914 0.900  0 0.00 0.100  0
#&gt; GSM28724     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28725     3   0.000      0.940 0.000  0 1.00 0.000  0
#&gt; GSM11263     3   0.000      0.940 0.000  0 1.00 0.000  0
#&gt; GSM11267     3   0.000      0.940 0.000  0 1.00 0.000  0
#&gt; GSM28744     1   0.029      0.962 0.992  0 0.00 0.008  0
#&gt; GSM28734     4   0.000      0.871 0.000  0 0.00 1.000  0
#&gt; GSM28747     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM11257     5   0.000      1.000 0.000  0 0.00 0.000  1
#&gt; GSM11252     4   0.202      0.916 0.100  0 0.00 0.900  0
#&gt; GSM11264     3   0.000      0.940 0.000  0 1.00 0.000  0
#&gt; GSM11247     3   0.327      0.675 0.000  0 0.78 0.220  0
#&gt; GSM11258     1   0.202      0.914 0.900  0 0.00 0.100  0
#&gt; GSM28728     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28746     1   0.191      0.918 0.908  0 0.00 0.092  0
#&gt; GSM28738     5   0.000      1.000 0.000  0 0.00 0.000  1
#&gt; GSM28741     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28729     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28742     4   0.207      0.914 0.104  0 0.00 0.896  0
#&gt; GSM11250     2   0.000      1.000 0.000  1 0.00 0.000  0
#&gt; GSM11245     4   0.213      0.910 0.108  0 0.00 0.892  0
#&gt; GSM11246     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM11261     4   0.179      0.916 0.084  0 0.00 0.916  0
#&gt; GSM11248     4   0.000      0.871 0.000  0 0.00 1.000  0
#&gt; GSM28732     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM11255     4   0.202      0.916 0.100  0 0.00 0.900  0
#&gt; GSM28731     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM28727     1   0.000      0.966 1.000  0 0.00 0.000  0
#&gt; GSM11251     1   0.000      0.966 1.000  0 0.00 0.000  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4 p5    p6
#&gt; GSM28735     1  0.0260      0.948 0.992  0 0.000 0.008  0 0.000
#&gt; GSM28736     1  0.0260      0.948 0.992  0 0.000 0.008  0 0.000
#&gt; GSM28737     1  0.2260      0.875 0.860  0 0.000 0.000  0 0.140
#&gt; GSM11249     6  0.0000      0.819 0.000  0 0.000 0.000  0 1.000
#&gt; GSM28745     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM11244     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM28748     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM11266     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM28730     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM11253     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM11254     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM11260     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM28733     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM11265     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
#&gt; GSM28739     1  0.2260      0.875 0.860  0 0.000 0.000  0 0.140
#&gt; GSM11243     4  0.0260      1.000 0.000  0 0.008 0.992  0 0.000
#&gt; GSM28740     1  0.2260      0.875 0.860  0 0.000 0.000  0 0.140
#&gt; GSM11259     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
#&gt; GSM28726     1  0.0260      0.948 0.992  0 0.000 0.008  0 0.000
#&gt; GSM28743     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
#&gt; GSM11256     1  0.2402      0.874 0.856  0 0.000 0.004  0 0.140
#&gt; GSM11262     1  0.2260      0.875 0.860  0 0.000 0.000  0 0.140
#&gt; GSM28724     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
#&gt; GSM28725     3  0.0000      1.000 0.000  0 1.000 0.000  0 0.000
#&gt; GSM11263     3  0.0000      1.000 0.000  0 1.000 0.000  0 0.000
#&gt; GSM11267     3  0.0000      1.000 0.000  0 1.000 0.000  0 0.000
#&gt; GSM28744     1  0.0363      0.946 0.988  0 0.000 0.000  0 0.012
#&gt; GSM28734     6  0.0146      0.819 0.004  0 0.000 0.000  0 0.996
#&gt; GSM28747     1  0.0146      0.949 0.996  0 0.000 0.004  0 0.000
#&gt; GSM11257     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM11252     6  0.2260      0.880 0.140  0 0.000 0.000  0 0.860
#&gt; GSM11264     3  0.0000      1.000 0.000  0 1.000 0.000  0 0.000
#&gt; GSM11247     4  0.0260      1.000 0.000  0 0.008 0.992  0 0.000
#&gt; GSM11258     1  0.2260      0.875 0.860  0 0.000 0.000  0 0.140
#&gt; GSM28728     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
#&gt; GSM28746     1  0.2178      0.881 0.868  0 0.000 0.000  0 0.132
#&gt; GSM28738     5  0.0000      1.000 0.000  0 0.000 0.000  1 0.000
#&gt; GSM28741     1  0.0260      0.948 0.992  0 0.000 0.008  0 0.000
#&gt; GSM28729     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
#&gt; GSM28742     6  0.2442      0.876 0.144  0 0.000 0.004  0 0.852
#&gt; GSM11250     2  0.0000      1.000 0.000  1 0.000 0.000  0 0.000
#&gt; GSM11245     6  0.2340      0.874 0.148  0 0.000 0.000  0 0.852
#&gt; GSM11246     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
#&gt; GSM11261     6  0.1910      0.879 0.108  0 0.000 0.000  0 0.892
#&gt; GSM11248     6  0.0000      0.819 0.000  0 0.000 0.000  0 1.000
#&gt; GSM28732     1  0.0146      0.949 0.996  0 0.000 0.004  0 0.000
#&gt; GSM11255     6  0.2260      0.880 0.140  0 0.000 0.000  0 0.860
#&gt; GSM28731     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
#&gt; GSM28727     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
#&gt; GSM11251     1  0.0000      0.950 1.000  0 0.000 0.000  0 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> ATC:pam 50     0.394 2
#> ATC:pam 50     0.370 3
#> ATC:pam 50     0.349 4
#> ATC:pam 50     0.331 5
#> ATC:pam 50     0.315 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.991       0.995         0.4678 0.530   0.530
#> 3 3 0.771           0.813       0.919         0.2633 0.776   0.610
#> 4 4 0.661           0.690       0.854         0.0894 0.848   0.664
#> 5 5 0.695           0.677       0.849         0.1334 0.883   0.686
#> 6 6 0.862           0.819       0.895         0.0592 0.916   0.715
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     1  0.0000      0.999 1.000 0.000
#&gt; GSM28736     1  0.0672      0.992 0.992 0.008
#&gt; GSM28737     1  0.0000      0.999 1.000 0.000
#&gt; GSM11249     1  0.0000      0.999 1.000 0.000
#&gt; GSM28745     2  0.0000      0.986 0.000 1.000
#&gt; GSM11244     2  0.0000      0.986 0.000 1.000
#&gt; GSM28748     2  0.0000      0.986 0.000 1.000
#&gt; GSM11266     2  0.0000      0.986 0.000 1.000
#&gt; GSM28730     2  0.0000      0.986 0.000 1.000
#&gt; GSM11253     2  0.0000      0.986 0.000 1.000
#&gt; GSM11254     2  0.0000      0.986 0.000 1.000
#&gt; GSM11260     2  0.0000      0.986 0.000 1.000
#&gt; GSM28733     2  0.0000      0.986 0.000 1.000
#&gt; GSM11265     1  0.0000      0.999 1.000 0.000
#&gt; GSM28739     1  0.0000      0.999 1.000 0.000
#&gt; GSM11243     2  0.0672      0.984 0.008 0.992
#&gt; GSM28740     1  0.0000      0.999 1.000 0.000
#&gt; GSM11259     1  0.0000      0.999 1.000 0.000
#&gt; GSM28726     1  0.0672      0.992 0.992 0.008
#&gt; GSM28743     1  0.0000      0.999 1.000 0.000
#&gt; GSM11256     1  0.0000      0.999 1.000 0.000
#&gt; GSM11262     1  0.0000      0.999 1.000 0.000
#&gt; GSM28724     1  0.0000      0.999 1.000 0.000
#&gt; GSM28725     2  0.0672      0.984 0.008 0.992
#&gt; GSM11263     2  0.0672      0.984 0.008 0.992
#&gt; GSM11267     2  0.0672      0.984 0.008 0.992
#&gt; GSM28744     1  0.0000      0.999 1.000 0.000
#&gt; GSM28734     1  0.0000      0.999 1.000 0.000
#&gt; GSM28747     1  0.0000      0.999 1.000 0.000
#&gt; GSM11257     1  0.0000      0.999 1.000 0.000
#&gt; GSM11252     1  0.0000      0.999 1.000 0.000
#&gt; GSM11264     2  0.0672      0.984 0.008 0.992
#&gt; GSM11247     2  0.0672      0.984 0.008 0.992
#&gt; GSM11258     1  0.0000      0.999 1.000 0.000
#&gt; GSM28728     1  0.0000      0.999 1.000 0.000
#&gt; GSM28746     1  0.0000      0.999 1.000 0.000
#&gt; GSM28738     1  0.0000      0.999 1.000 0.000
#&gt; GSM28741     2  0.4562      0.902 0.096 0.904
#&gt; GSM28729     1  0.0000      0.999 1.000 0.000
#&gt; GSM28742     1  0.0000      0.999 1.000 0.000
#&gt; GSM11250     2  0.0000      0.986 0.000 1.000
#&gt; GSM11245     1  0.0000      0.999 1.000 0.000
#&gt; GSM11246     1  0.0000      0.999 1.000 0.000
#&gt; GSM11261     2  0.4562      0.902 0.096 0.904
#&gt; GSM11248     1  0.0000      0.999 1.000 0.000
#&gt; GSM28732     1  0.0000      0.999 1.000 0.000
#&gt; GSM11255     1  0.0000      0.999 1.000 0.000
#&gt; GSM28731     1  0.0000      0.999 1.000 0.000
#&gt; GSM28727     1  0.0000      0.999 1.000 0.000
#&gt; GSM11251     1  0.0000      0.999 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1  0.0829     0.9188 0.984 0.004 0.012
#&gt; GSM28736     1  0.5746     0.6740 0.780 0.180 0.040
#&gt; GSM28737     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM11249     3  0.5785     0.6502 0.332 0.000 0.668
#&gt; GSM28745     2  0.0000     0.9506 0.000 1.000 0.000
#&gt; GSM11244     2  0.0000     0.9506 0.000 1.000 0.000
#&gt; GSM28748     2  0.0237     0.9491 0.000 0.996 0.004
#&gt; GSM11266     2  0.0237     0.9491 0.000 0.996 0.004
#&gt; GSM28730     2  0.0000     0.9506 0.000 1.000 0.000
#&gt; GSM11253     2  0.0000     0.9506 0.000 1.000 0.000
#&gt; GSM11254     2  0.0000     0.9506 0.000 1.000 0.000
#&gt; GSM11260     2  0.0000     0.9506 0.000 1.000 0.000
#&gt; GSM28733     2  0.0000     0.9506 0.000 1.000 0.000
#&gt; GSM11265     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM28739     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM11243     3  0.3752     0.6029 0.000 0.144 0.856
#&gt; GSM28740     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM11259     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM28726     1  0.6603     0.4433 0.648 0.020 0.332
#&gt; GSM28743     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM11256     3  0.6280     0.4188 0.460 0.000 0.540
#&gt; GSM11262     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM28724     1  0.0592     0.9203 0.988 0.000 0.012
#&gt; GSM28725     3  0.0000     0.7039 0.000 0.000 1.000
#&gt; GSM11263     3  0.0000     0.7039 0.000 0.000 1.000
#&gt; GSM11267     3  0.0000     0.7039 0.000 0.000 1.000
#&gt; GSM28744     1  0.0892     0.9132 0.980 0.000 0.020
#&gt; GSM28734     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM28747     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM11257     3  0.5882     0.6343 0.348 0.000 0.652
#&gt; GSM11252     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM11264     3  0.0000     0.7039 0.000 0.000 1.000
#&gt; GSM11247     3  0.3879     0.5939 0.000 0.152 0.848
#&gt; GSM11258     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM28728     1  0.0592     0.9203 0.988 0.000 0.012
#&gt; GSM28746     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM28738     3  0.5882     0.6343 0.348 0.000 0.652
#&gt; GSM28741     2  0.5760     0.4422 0.328 0.672 0.000
#&gt; GSM28729     1  0.0592     0.9203 0.988 0.000 0.012
#&gt; GSM28742     1  0.6057     0.4519 0.656 0.004 0.340
#&gt; GSM11250     2  0.0237     0.9491 0.000 0.996 0.004
#&gt; GSM11245     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM11246     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM11261     1  0.9792    -0.0732 0.408 0.240 0.352
#&gt; GSM11248     3  0.5810     0.6472 0.336 0.000 0.664
#&gt; GSM28732     1  0.0829     0.9188 0.984 0.004 0.012
#&gt; GSM11255     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM28731     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM28727     1  0.0000     0.9274 1.000 0.000 0.000
#&gt; GSM11251     1  0.0000     0.9274 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.4804      0.196 0.616 0.000 0.000 0.384
#&gt; GSM28736     1  0.4964      0.180 0.616 0.004 0.000 0.380
#&gt; GSM28737     1  0.0188      0.821 0.996 0.000 0.000 0.004
#&gt; GSM11249     1  0.7513     -0.336 0.492 0.000 0.224 0.284
#&gt; GSM28745     2  0.0188      0.915 0.000 0.996 0.000 0.004
#&gt; GSM11244     2  0.0000      0.915 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.4663      0.637 0.000 0.716 0.012 0.272
#&gt; GSM11266     2  0.0592      0.910 0.000 0.984 0.000 0.016
#&gt; GSM28730     2  0.0188      0.915 0.000 0.996 0.000 0.004
#&gt; GSM11253     2  0.0000      0.915 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.915 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.915 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0469      0.910 0.000 0.988 0.000 0.012
#&gt; GSM11265     1  0.1211      0.809 0.960 0.000 0.000 0.040
#&gt; GSM28739     1  0.0000      0.822 1.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.2676      0.871 0.000 0.012 0.896 0.092
#&gt; GSM28740     1  0.0000      0.822 1.000 0.000 0.000 0.000
#&gt; GSM11259     1  0.0000      0.822 1.000 0.000 0.000 0.000
#&gt; GSM28726     4  0.7909      0.312 0.420 0.080 0.060 0.440
#&gt; GSM28743     1  0.0000      0.822 1.000 0.000 0.000 0.000
#&gt; GSM11256     4  0.6091      0.473 0.344 0.000 0.060 0.596
#&gt; GSM11262     1  0.0188      0.821 0.996 0.000 0.000 0.004
#&gt; GSM28724     1  0.2973      0.695 0.856 0.000 0.000 0.144
#&gt; GSM28725     3  0.0000      0.929 0.000 0.000 1.000 0.000
#&gt; GSM11263     3  0.0000      0.929 0.000 0.000 1.000 0.000
#&gt; GSM11267     3  0.0000      0.929 0.000 0.000 1.000 0.000
#&gt; GSM28744     1  0.0672      0.817 0.984 0.000 0.008 0.008
#&gt; GSM28734     1  0.3400      0.554 0.820 0.000 0.000 0.180
#&gt; GSM28747     1  0.1389      0.798 0.952 0.000 0.000 0.048
#&gt; GSM11257     4  0.6037      0.499 0.304 0.000 0.068 0.628
#&gt; GSM11252     1  0.0188      0.821 0.996 0.000 0.000 0.004
#&gt; GSM11264     3  0.0000      0.929 0.000 0.000 1.000 0.000
#&gt; GSM11247     3  0.3937      0.759 0.000 0.012 0.800 0.188
#&gt; GSM11258     1  0.1557      0.789 0.944 0.000 0.000 0.056
#&gt; GSM28728     1  0.1716      0.785 0.936 0.000 0.000 0.064
#&gt; GSM28746     1  0.0000      0.822 1.000 0.000 0.000 0.000
#&gt; GSM28738     4  0.6016      0.498 0.300 0.000 0.068 0.632
#&gt; GSM28741     4  0.8351      0.429 0.300 0.268 0.020 0.412
#&gt; GSM28729     1  0.2011      0.768 0.920 0.000 0.000 0.080
#&gt; GSM28742     4  0.8176      0.475 0.344 0.048 0.132 0.476
#&gt; GSM11250     2  0.6600      0.445 0.084 0.620 0.012 0.284
#&gt; GSM11245     1  0.0188      0.821 0.996 0.000 0.000 0.004
#&gt; GSM11246     1  0.0469      0.819 0.988 0.000 0.000 0.012
#&gt; GSM11261     4  0.8087      0.471 0.320 0.012 0.236 0.432
#&gt; GSM11248     1  0.5925      0.101 0.648 0.000 0.068 0.284
#&gt; GSM28732     1  0.4916      0.095 0.576 0.000 0.000 0.424
#&gt; GSM11255     1  0.2530      0.690 0.888 0.000 0.000 0.112
#&gt; GSM28731     1  0.0188      0.821 0.996 0.000 0.000 0.004
#&gt; GSM28727     1  0.1557      0.794 0.944 0.000 0.000 0.056
#&gt; GSM11251     1  0.0469      0.819 0.988 0.000 0.000 0.012
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.6638     0.2769 0.320 0.000 0.000 0.240 0.440
#&gt; GSM28736     1  0.6677     0.2097 0.540 0.000 0.020 0.244 0.196
#&gt; GSM28737     1  0.0290     0.8478 0.992 0.000 0.000 0.000 0.008
#&gt; GSM11249     4  0.6605     0.4137 0.252 0.000 0.000 0.460 0.288
#&gt; GSM28745     2  0.0000     0.9485 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9485 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.4657     0.4554 0.000 0.668 0.000 0.036 0.296
#&gt; GSM11266     2  0.0000     0.9485 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000     0.9485 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000     0.9485 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000     0.9485 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9485 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9485 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.5568     0.3083 0.596 0.000 0.000 0.096 0.308
#&gt; GSM28739     1  0.0000     0.8480 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11243     3  0.4843     0.6800 0.000 0.000 0.660 0.048 0.292
#&gt; GSM28740     1  0.0290     0.8475 0.992 0.000 0.000 0.008 0.000
#&gt; GSM11259     1  0.0404     0.8466 0.988 0.000 0.000 0.000 0.012
#&gt; GSM28726     5  0.4173     0.3921 0.300 0.012 0.000 0.000 0.688
#&gt; GSM28743     1  0.0162     0.8484 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11256     4  0.0290     0.6508 0.008 0.000 0.000 0.992 0.000
#&gt; GSM11262     1  0.0162     0.8484 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28724     1  0.3635     0.6291 0.748 0.000 0.000 0.248 0.004
#&gt; GSM28725     3  0.0000     0.7919 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11263     3  0.0000     0.7919 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0000     0.7919 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28744     1  0.0955     0.8339 0.968 0.000 0.000 0.028 0.004
#&gt; GSM28734     1  0.3039     0.6519 0.836 0.000 0.000 0.012 0.152
#&gt; GSM28747     1  0.3684     0.5205 0.720 0.000 0.000 0.000 0.280
#&gt; GSM11257     4  0.0000     0.6542 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11252     1  0.0000     0.8480 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11264     3  0.0000     0.7919 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.4843     0.6800 0.000 0.000 0.660 0.048 0.292
#&gt; GSM11258     1  0.3274     0.6643 0.780 0.000 0.000 0.220 0.000
#&gt; GSM28728     1  0.2629     0.7562 0.860 0.000 0.000 0.136 0.004
#&gt; GSM28746     1  0.0162     0.8482 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28738     4  0.0000     0.6542 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28741     5  0.6124     0.0927 0.052 0.344 0.004 0.036 0.564
#&gt; GSM28729     1  0.3689     0.6220 0.740 0.000 0.000 0.256 0.004
#&gt; GSM28742     5  0.2790     0.2038 0.052 0.000 0.000 0.068 0.880
#&gt; GSM11250     5  0.5096    -0.1842 0.000 0.444 0.000 0.036 0.520
#&gt; GSM11245     1  0.0000     0.8480 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11246     1  0.0162     0.8475 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11261     3  0.5772     0.6165 0.000 0.028 0.580 0.048 0.344
#&gt; GSM11248     4  0.6975     0.4229 0.236 0.000 0.016 0.460 0.288
#&gt; GSM28732     5  0.6275     0.3528 0.300 0.000 0.000 0.180 0.520
#&gt; GSM11255     1  0.0000     0.8480 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28731     1  0.0162     0.8484 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28727     1  0.3730     0.5107 0.712 0.000 0.000 0.000 0.288
#&gt; GSM11251     1  0.0290     0.8479 0.992 0.000 0.000 0.000 0.008
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.2890      0.803 0.108 0.000 0.000 0.020 0.856 0.016
#&gt; GSM28736     1  0.2445      0.902 0.896 0.000 0.000 0.020 0.056 0.028
#&gt; GSM28737     1  0.0260      0.947 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM11249     6  0.3915      0.278 0.004 0.000 0.000 0.412 0.000 0.584
#&gt; GSM28745     2  0.0000      0.899 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000      0.899 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.4838      0.477 0.000 0.564 0.000 0.000 0.064 0.372
#&gt; GSM11266     2  0.0000      0.899 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28730     2  0.0000      0.899 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0000      0.899 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11254     2  0.0000      0.899 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.899 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000      0.899 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     5  0.2696      0.828 0.116 0.000 0.000 0.000 0.856 0.028
#&gt; GSM28739     1  0.0993      0.943 0.964 0.000 0.000 0.000 0.012 0.024
#&gt; GSM11243     3  0.2378      0.846 0.000 0.000 0.848 0.000 0.000 0.152
#&gt; GSM28740     1  0.0790      0.941 0.968 0.000 0.000 0.000 0.000 0.032
#&gt; GSM11259     1  0.0806      0.946 0.972 0.000 0.000 0.000 0.008 0.020
#&gt; GSM28726     5  0.2364      0.824 0.072 0.000 0.004 0.000 0.892 0.032
#&gt; GSM28743     1  0.0146      0.948 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM11256     4  0.0458      0.974 0.016 0.000 0.000 0.984 0.000 0.000
#&gt; GSM11262     1  0.0692      0.945 0.976 0.000 0.000 0.000 0.004 0.020
#&gt; GSM28724     1  0.1851      0.928 0.928 0.000 0.000 0.012 0.024 0.036
#&gt; GSM28725     3  0.0000      0.929 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11263     3  0.0000      0.929 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000      0.929 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     1  0.4045      0.540 0.648 0.000 0.000 0.008 0.008 0.336
#&gt; GSM28734     1  0.1866      0.902 0.908 0.000 0.000 0.000 0.008 0.084
#&gt; GSM28747     5  0.2624      0.821 0.124 0.000 0.000 0.000 0.856 0.020
#&gt; GSM11257     4  0.0363      0.978 0.012 0.000 0.000 0.988 0.000 0.000
#&gt; GSM11252     1  0.0806      0.945 0.972 0.000 0.000 0.000 0.008 0.020
#&gt; GSM11264     3  0.0000      0.929 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     3  0.2378      0.846 0.000 0.000 0.848 0.000 0.000 0.152
#&gt; GSM11258     1  0.1049      0.940 0.960 0.000 0.000 0.008 0.000 0.032
#&gt; GSM28728     1  0.1821      0.929 0.928 0.000 0.000 0.008 0.024 0.040
#&gt; GSM28746     1  0.0000      0.947 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28738     4  0.0000      0.964 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28741     5  0.5466      0.117 0.008 0.072 0.004 0.004 0.520 0.392
#&gt; GSM28729     1  0.1966      0.924 0.924 0.000 0.000 0.024 0.024 0.028
#&gt; GSM28742     5  0.2651      0.736 0.028 0.000 0.000 0.000 0.860 0.112
#&gt; GSM11250     2  0.5009      0.436 0.000 0.536 0.000 0.000 0.076 0.388
#&gt; GSM11245     1  0.0806      0.945 0.972 0.000 0.000 0.000 0.008 0.020
#&gt; GSM11246     1  0.0146      0.948 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM11261     6  0.7074     -0.175 0.112 0.000 0.132 0.004 0.344 0.408
#&gt; GSM11248     6  0.3915      0.278 0.004 0.000 0.000 0.412 0.000 0.584
#&gt; GSM28732     5  0.1732      0.827 0.072 0.000 0.000 0.004 0.920 0.004
#&gt; GSM11255     1  0.1265      0.934 0.948 0.000 0.000 0.000 0.008 0.044
#&gt; GSM28731     1  0.0790      0.941 0.968 0.000 0.000 0.000 0.000 0.032
#&gt; GSM28727     5  0.2398      0.833 0.104 0.000 0.000 0.000 0.876 0.020
#&gt; GSM11251     1  0.0405      0.947 0.988 0.000 0.000 0.000 0.008 0.004
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:mclust 50     0.394 2
#> ATC:mclust 45     0.421 3
#> ATC:mclust 37     0.413 4
#> ATC:mclust 39     0.521 5
#> ATC:mclust 44     0.495 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21342 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.486           0.796       0.893         0.4524 0.556   0.556
#> 3 3 1.000           0.951       0.981         0.3255 0.591   0.406
#> 4 4 0.737           0.751       0.841         0.1698 0.864   0.692
#> 5 5 0.785           0.890       0.923         0.0940 0.897   0.690
#> 6 6 0.761           0.741       0.858         0.0315 0.987   0.943
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28735     2  0.4690      0.837 0.100 0.900
#&gt; GSM28736     2  0.0000      0.844 0.000 1.000
#&gt; GSM28737     2  0.1633      0.845 0.024 0.976
#&gt; GSM11249     1  0.0000      0.940 1.000 0.000
#&gt; GSM28745     2  0.0376      0.844 0.004 0.996
#&gt; GSM11244     2  0.0376      0.844 0.004 0.996
#&gt; GSM28748     2  0.0376      0.844 0.004 0.996
#&gt; GSM11266     2  0.0376      0.844 0.004 0.996
#&gt; GSM28730     2  0.0376      0.844 0.004 0.996
#&gt; GSM11253     2  0.0376      0.844 0.004 0.996
#&gt; GSM11254     2  0.0376      0.844 0.004 0.996
#&gt; GSM11260     2  0.0376      0.844 0.004 0.996
#&gt; GSM28733     2  0.0376      0.844 0.004 0.996
#&gt; GSM11265     2  1.0000      0.277 0.496 0.504
#&gt; GSM28739     1  0.8081      0.554 0.752 0.248
#&gt; GSM11243     1  0.0000      0.940 1.000 0.000
#&gt; GSM28740     2  0.8608      0.720 0.284 0.716
#&gt; GSM11259     2  0.0938      0.845 0.012 0.988
#&gt; GSM28726     2  0.0000      0.844 0.000 1.000
#&gt; GSM28743     2  0.8443      0.732 0.272 0.728
#&gt; GSM11256     2  0.8267      0.743 0.260 0.740
#&gt; GSM11262     2  0.4161      0.839 0.084 0.916
#&gt; GSM28724     2  0.8608      0.720 0.284 0.716
#&gt; GSM28725     1  0.0000      0.940 1.000 0.000
#&gt; GSM11263     1  0.0000      0.940 1.000 0.000
#&gt; GSM11267     1  0.0000      0.940 1.000 0.000
#&gt; GSM28744     1  0.0376      0.939 0.996 0.004
#&gt; GSM28734     1  0.0376      0.939 0.996 0.004
#&gt; GSM28747     2  0.7139      0.793 0.196 0.804
#&gt; GSM11257     2  0.5408      0.831 0.124 0.876
#&gt; GSM11252     1  0.0376      0.939 0.996 0.004
#&gt; GSM11264     1  0.0000      0.940 1.000 0.000
#&gt; GSM11247     1  0.0000      0.940 1.000 0.000
#&gt; GSM11258     1  0.9850     -0.056 0.572 0.428
#&gt; GSM28728     2  0.9358      0.626 0.352 0.648
#&gt; GSM28746     2  0.9491      0.598 0.368 0.632
#&gt; GSM28738     2  0.0000      0.844 0.000 1.000
#&gt; GSM28741     2  0.0376      0.844 0.004 0.996
#&gt; GSM28729     2  0.5946      0.822 0.144 0.856
#&gt; GSM28742     2  0.9998      0.294 0.492 0.508
#&gt; GSM11250     2  0.0376      0.844 0.004 0.996
#&gt; GSM11245     1  0.0376      0.939 0.996 0.004
#&gt; GSM11246     2  0.8909      0.691 0.308 0.692
#&gt; GSM11261     1  0.0000      0.940 1.000 0.000
#&gt; GSM11248     1  0.0376      0.939 0.996 0.004
#&gt; GSM28732     2  0.9170      0.658 0.332 0.668
#&gt; GSM11255     1  0.0376      0.939 0.996 0.004
#&gt; GSM28731     2  0.6973      0.798 0.188 0.812
#&gt; GSM28727     2  0.5737      0.826 0.136 0.864
#&gt; GSM11251     2  0.5059      0.834 0.112 0.888
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28735     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28736     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28737     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11249     3   0.000     0.9946 0.000 0.000 1.000
#&gt; GSM28745     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM11244     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM28748     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM11266     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM28730     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM11253     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM11254     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM11260     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM28733     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM11265     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28739     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11243     3   0.000     0.9946 0.000 0.000 1.000
#&gt; GSM28740     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11259     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28726     1   0.553     0.5824 0.704 0.296 0.000
#&gt; GSM28743     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11256     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11262     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28724     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28725     3   0.000     0.9946 0.000 0.000 1.000
#&gt; GSM11263     3   0.000     0.9946 0.000 0.000 1.000
#&gt; GSM11267     3   0.000     0.9946 0.000 0.000 1.000
#&gt; GSM28744     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28734     1   0.630     0.0853 0.516 0.000 0.484
#&gt; GSM28747     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11257     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11252     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11264     3   0.000     0.9946 0.000 0.000 1.000
#&gt; GSM11247     3   0.000     0.9946 0.000 0.000 1.000
#&gt; GSM11258     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28728     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28746     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28738     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28741     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM28729     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28742     3   0.215     0.9493 0.016 0.036 0.948
#&gt; GSM11250     2   0.000     1.0000 0.000 1.000 0.000
#&gt; GSM11245     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11246     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11261     3   0.000     0.9946 0.000 0.000 1.000
#&gt; GSM11248     3   0.000     0.9946 0.000 0.000 1.000
#&gt; GSM28732     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11255     1   0.355     0.8325 0.868 0.000 0.132
#&gt; GSM28731     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM28727     1   0.000     0.9669 1.000 0.000 0.000
#&gt; GSM11251     1   0.000     0.9669 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28735     1  0.4647    0.58478 0.704 0.000 0.008 0.288
#&gt; GSM28736     1  0.3074    0.66765 0.848 0.000 0.000 0.152
#&gt; GSM28737     1  0.3942    0.39808 0.764 0.000 0.000 0.236
#&gt; GSM11249     3  0.1022    0.96225 0.000 0.000 0.968 0.032
#&gt; GSM28745     2  0.0000    0.99690 0.000 1.000 0.000 0.000
#&gt; GSM11244     2  0.0000    0.99690 0.000 1.000 0.000 0.000
#&gt; GSM28748     2  0.0000    0.99690 0.000 1.000 0.000 0.000
#&gt; GSM11266     2  0.0000    0.99690 0.000 1.000 0.000 0.000
#&gt; GSM28730     2  0.0000    0.99690 0.000 1.000 0.000 0.000
#&gt; GSM11253     2  0.0000    0.99690 0.000 1.000 0.000 0.000
#&gt; GSM11254     2  0.0000    0.99690 0.000 1.000 0.000 0.000
#&gt; GSM11260     2  0.0000    0.99690 0.000 1.000 0.000 0.000
#&gt; GSM28733     2  0.0000    0.99690 0.000 1.000 0.000 0.000
#&gt; GSM11265     1  0.1474    0.68501 0.948 0.000 0.000 0.052
#&gt; GSM28739     1  0.1389    0.66061 0.952 0.000 0.000 0.048
#&gt; GSM11243     3  0.0592    0.96605 0.000 0.000 0.984 0.016
#&gt; GSM28740     1  0.4661   -0.03666 0.652 0.000 0.000 0.348
#&gt; GSM11259     1  0.0000    0.68081 1.000 0.000 0.000 0.000
#&gt; GSM28726     1  0.4304    0.59304 0.716 0.000 0.000 0.284
#&gt; GSM28743     1  0.3219    0.54332 0.836 0.000 0.000 0.164
#&gt; GSM11256     4  0.4661    0.90656 0.348 0.000 0.000 0.652
#&gt; GSM11262     1  0.4164    0.31808 0.736 0.000 0.000 0.264
#&gt; GSM28724     1  0.3024    0.56616 0.852 0.000 0.000 0.148
#&gt; GSM28725     3  0.0188    0.96860 0.000 0.000 0.996 0.004
#&gt; GSM11263     3  0.0592    0.96751 0.000 0.000 0.984 0.016
#&gt; GSM11267     3  0.0188    0.96860 0.000 0.000 0.996 0.004
#&gt; GSM28744     4  0.5167    0.67931 0.488 0.000 0.004 0.508
#&gt; GSM28734     3  0.2676    0.85652 0.092 0.000 0.896 0.012
#&gt; GSM28747     1  0.4072    0.61749 0.748 0.000 0.000 0.252
#&gt; GSM11257     4  0.4624    0.90805 0.340 0.000 0.000 0.660
#&gt; GSM11252     1  0.3401    0.56187 0.840 0.000 0.008 0.152
#&gt; GSM11264     3  0.0592    0.96751 0.000 0.000 0.984 0.016
#&gt; GSM11247     3  0.0707    0.96485 0.000 0.000 0.980 0.020
#&gt; GSM11258     1  0.4624    0.00668 0.660 0.000 0.000 0.340
#&gt; GSM28728     1  0.3942    0.62719 0.764 0.000 0.000 0.236
#&gt; GSM28746     1  0.3444    0.50957 0.816 0.000 0.000 0.184
#&gt; GSM28738     4  0.4781    0.90491 0.336 0.004 0.000 0.660
#&gt; GSM28741     2  0.1042    0.97279 0.008 0.972 0.000 0.020
#&gt; GSM28729     1  0.3074    0.67193 0.848 0.000 0.000 0.152
#&gt; GSM28742     1  0.5878    0.51075 0.632 0.000 0.056 0.312
#&gt; GSM11250     2  0.0188    0.99408 0.000 0.996 0.000 0.004
#&gt; GSM11245     1  0.1488    0.67722 0.956 0.000 0.012 0.032
#&gt; GSM11246     1  0.0592    0.68333 0.984 0.000 0.000 0.016
#&gt; GSM11261     3  0.0336    0.96851 0.000 0.000 0.992 0.008
#&gt; GSM11248     3  0.2081    0.93084 0.000 0.000 0.916 0.084
#&gt; GSM28732     1  0.5344    0.55117 0.668 0.000 0.032 0.300
#&gt; GSM11255     1  0.3335    0.62909 0.860 0.000 0.120 0.020
#&gt; GSM28731     1  0.2011    0.63785 0.920 0.000 0.000 0.080
#&gt; GSM28727     1  0.3486    0.65320 0.812 0.000 0.000 0.188
#&gt; GSM11251     1  0.1022    0.66984 0.968 0.000 0.000 0.032
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28735     5  0.1628      0.923 0.056 0.000 0.000 0.008 0.936
#&gt; GSM28736     5  0.3106      0.853 0.140 0.000 0.000 0.020 0.840
#&gt; GSM28737     1  0.0865      0.888 0.972 0.000 0.000 0.024 0.004
#&gt; GSM11249     3  0.0290      0.913 0.000 0.000 0.992 0.000 0.008
#&gt; GSM28745     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0162      0.993 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28748     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11266     2  0.0290      0.992 0.000 0.992 0.000 0.008 0.000
#&gt; GSM28730     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11253     2  0.0404      0.987 0.000 0.988 0.000 0.012 0.000
#&gt; GSM11254     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11260     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0290      0.992 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11265     1  0.1830      0.891 0.924 0.000 0.000 0.008 0.068
#&gt; GSM28739     1  0.0290      0.893 0.992 0.000 0.000 0.000 0.008
#&gt; GSM11243     3  0.1430      0.886 0.000 0.000 0.944 0.004 0.052
#&gt; GSM28740     1  0.0963      0.880 0.964 0.000 0.000 0.036 0.000
#&gt; GSM11259     1  0.3353      0.811 0.796 0.000 0.000 0.008 0.196
#&gt; GSM28726     5  0.1670      0.922 0.052 0.000 0.000 0.012 0.936
#&gt; GSM28743     1  0.0693      0.891 0.980 0.000 0.000 0.008 0.012
#&gt; GSM11256     4  0.3123      0.881 0.160 0.000 0.000 0.828 0.012
#&gt; GSM11262     1  0.0912      0.884 0.972 0.000 0.000 0.016 0.012
#&gt; GSM28724     1  0.1195      0.887 0.960 0.000 0.000 0.028 0.012
#&gt; GSM28725     3  0.0290      0.913 0.000 0.000 0.992 0.008 0.000
#&gt; GSM11263     3  0.0000      0.913 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11267     3  0.0162      0.913 0.000 0.000 0.996 0.004 0.000
#&gt; GSM28744     4  0.4136      0.703 0.048 0.000 0.000 0.764 0.188
#&gt; GSM28734     3  0.3759      0.646 0.220 0.000 0.764 0.000 0.016
#&gt; GSM28747     1  0.4026      0.748 0.736 0.000 0.000 0.020 0.244
#&gt; GSM11257     4  0.2424      0.897 0.132 0.000 0.000 0.868 0.000
#&gt; GSM11252     1  0.0579      0.893 0.984 0.000 0.000 0.008 0.008
#&gt; GSM11264     3  0.0000      0.913 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11247     3  0.4016      0.634 0.000 0.000 0.716 0.012 0.272
#&gt; GSM11258     1  0.0912      0.883 0.972 0.000 0.000 0.016 0.012
#&gt; GSM28728     5  0.3410      0.871 0.092 0.000 0.000 0.068 0.840
#&gt; GSM28746     1  0.0566      0.889 0.984 0.000 0.000 0.012 0.004
#&gt; GSM28738     4  0.2389      0.891 0.116 0.004 0.000 0.880 0.000
#&gt; GSM28741     2  0.1116      0.973 0.004 0.964 0.000 0.028 0.004
#&gt; GSM28729     5  0.3339      0.875 0.112 0.000 0.000 0.048 0.840
#&gt; GSM28742     5  0.1267      0.894 0.024 0.000 0.004 0.012 0.960
#&gt; GSM11250     2  0.0404      0.991 0.000 0.988 0.000 0.012 0.000
#&gt; GSM11245     1  0.1768      0.890 0.924 0.000 0.004 0.000 0.072
#&gt; GSM11246     1  0.2605      0.858 0.852 0.000 0.000 0.000 0.148
#&gt; GSM11261     3  0.0451      0.912 0.004 0.000 0.988 0.000 0.008
#&gt; GSM11248     3  0.2140      0.875 0.040 0.000 0.924 0.024 0.012
#&gt; GSM28732     5  0.1270      0.921 0.052 0.000 0.000 0.000 0.948
#&gt; GSM11255     1  0.4064      0.804 0.792 0.000 0.116 0.000 0.092
#&gt; GSM28731     1  0.2813      0.874 0.868 0.000 0.000 0.024 0.108
#&gt; GSM28727     1  0.3906      0.759 0.744 0.000 0.000 0.016 0.240
#&gt; GSM11251     1  0.3061      0.858 0.844 0.000 0.000 0.020 0.136
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28735     5  0.2647     0.7206 0.088 0.000 0.000 0.016 0.876 0.020
#&gt; GSM28736     5  0.4726     0.6319 0.140 0.000 0.000 0.100 0.728 0.032
#&gt; GSM28737     1  0.1226     0.8525 0.952 0.000 0.000 0.040 0.004 0.004
#&gt; GSM11249     3  0.0767     0.7969 0.000 0.000 0.976 0.008 0.004 0.012
#&gt; GSM28745     2  0.0000     0.9915 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11244     2  0.0000     0.9915 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28748     2  0.0146     0.9906 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11266     2  0.0146     0.9905 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM28730     2  0.0146     0.9908 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM11253     2  0.0260     0.9891 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM11254     2  0.0146     0.9908 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM11260     2  0.0000     0.9915 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28733     2  0.0000     0.9915 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11265     1  0.1333     0.8506 0.944 0.000 0.000 0.000 0.048 0.008
#&gt; GSM28739     1  0.1194     0.8533 0.956 0.000 0.000 0.032 0.008 0.004
#&gt; GSM11243     3  0.3619     0.4330 0.000 0.000 0.744 0.000 0.024 0.232
#&gt; GSM28740     1  0.1462     0.8470 0.936 0.000 0.000 0.056 0.000 0.008
#&gt; GSM11259     1  0.3539     0.7384 0.768 0.000 0.000 0.008 0.208 0.016
#&gt; GSM28726     5  0.2088     0.7140 0.068 0.000 0.000 0.000 0.904 0.028
#&gt; GSM28743     1  0.1511     0.8490 0.940 0.000 0.000 0.012 0.004 0.044
#&gt; GSM11256     4  0.2492     0.5661 0.100 0.000 0.000 0.876 0.004 0.020
#&gt; GSM11262     1  0.1777     0.8476 0.928 0.000 0.000 0.024 0.004 0.044
#&gt; GSM28724     1  0.2755     0.8100 0.844 0.000 0.000 0.004 0.012 0.140
#&gt; GSM28725     3  0.0146     0.8006 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11263     3  0.0000     0.8020 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11267     3  0.0000     0.8020 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28744     4  0.4513     0.4425 0.032 0.000 0.000 0.732 0.180 0.056
#&gt; GSM28734     3  0.5794     0.3037 0.244 0.000 0.580 0.156 0.004 0.016
#&gt; GSM28747     1  0.3646     0.7605 0.776 0.000 0.000 0.000 0.172 0.052
#&gt; GSM11257     4  0.4165     0.6597 0.036 0.000 0.000 0.672 0.000 0.292
#&gt; GSM11252     1  0.1439     0.8562 0.952 0.000 0.012 0.012 0.016 0.008
#&gt; GSM11264     3  0.0000     0.8020 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11247     6  0.5787     0.1381 0.000 0.000 0.424 0.012 0.124 0.440
#&gt; GSM11258     1  0.2501     0.8112 0.872 0.000 0.000 0.108 0.004 0.016
#&gt; GSM28728     6  0.6107     0.0558 0.044 0.000 0.000 0.128 0.292 0.536
#&gt; GSM28746     1  0.2102     0.8369 0.908 0.000 0.000 0.068 0.012 0.012
#&gt; GSM28738     4  0.4047     0.6585 0.028 0.000 0.000 0.676 0.000 0.296
#&gt; GSM28741     2  0.1684     0.9429 0.008 0.940 0.000 0.008 0.016 0.028
#&gt; GSM28729     5  0.6956     0.0370 0.064 0.000 0.000 0.352 0.360 0.224
#&gt; GSM28742     5  0.1668     0.6109 0.008 0.000 0.000 0.004 0.928 0.060
#&gt; GSM11250     2  0.0291     0.9887 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; GSM11245     1  0.2315     0.8462 0.892 0.000 0.000 0.016 0.084 0.008
#&gt; GSM11246     1  0.1918     0.8381 0.904 0.000 0.000 0.000 0.088 0.008
#&gt; GSM11261     3  0.0748     0.7968 0.000 0.000 0.976 0.004 0.004 0.016
#&gt; GSM11248     3  0.5052     0.4230 0.064 0.000 0.640 0.276 0.004 0.016
#&gt; GSM28732     5  0.3009     0.7073 0.112 0.000 0.000 0.004 0.844 0.040
#&gt; GSM11255     1  0.4404     0.7118 0.732 0.000 0.172 0.004 0.088 0.004
#&gt; GSM28731     1  0.4696     0.5635 0.660 0.000 0.000 0.276 0.048 0.016
#&gt; GSM28727     1  0.4327     0.6310 0.688 0.000 0.000 0.004 0.260 0.048
#&gt; GSM11251     1  0.3351     0.8070 0.832 0.000 0.000 0.016 0.104 0.048
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> ATC:NMF 47     0.391 2
#> ATC:NMF 49     0.368 3
#> ATC:NMF 46     0.412 4
#> ATC:NMF 50     0.479 5
#> ATC:NMF 43     0.493 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] genefilter_1.66.0    ComplexHeatmap_2.3.1 markdown_1.1         knitr_1.26          
#> [5] GetoptLong_0.1.7     cola_1.3.2          
#> 
#> loaded via a namespace (and not attached):
#>  [1] circlize_0.4.8       shape_1.4.4          xfun_0.11            slam_0.1-46         
#>  [5] lattice_0.20-38      splines_3.6.0        colorspace_1.4-1     vctrs_0.2.0         
#>  [9] stats4_3.6.0         blob_1.2.0           XML_3.98-1.20        survival_2.44-1.1   
#> [13] rlang_0.4.2          pillar_1.4.2         DBI_1.0.0            BiocGenerics_0.30.0 
#> [17] bit64_0.9-7          RColorBrewer_1.1-2   matrixStats_0.55.0   stringr_1.4.0       
#> [21] GlobalOptions_0.1.1  evaluate_0.14        memoise_1.1.0        Biobase_2.44.0      
#> [25] IRanges_2.18.3       parallel_3.6.0       AnnotationDbi_1.46.1 highr_0.8           
#> [29] Rcpp_1.0.3           xtable_1.8-4         backports_1.1.5      S4Vectors_0.22.1    
#> [33] annotate_1.62.0      skmeans_0.2-11       bit_1.1-14           microbenchmark_1.4-7
#> [37] brew_1.0-6           impute_1.58.0        rjson_0.2.20         png_0.1-7           
#> [41] digest_0.6.23        stringi_1.4.3        polyclip_1.10-0      clue_0.3-57         
#> [45] tools_3.6.0          bitops_1.0-6         magrittr_1.5         eulerr_6.0.0        
#> [49] RCurl_1.95-4.12      RSQLite_2.1.4        tibble_2.1.3         cluster_2.1.0       
#> [53] crayon_1.3.4         pkgconfig_2.0.3      zeallot_0.1.0        Matrix_1.2-17       
#> [57] xml2_1.2.2           httr_1.4.1           R6_2.4.1             mclust_5.4.5        
#> [61] compiler_3.6.0
```


