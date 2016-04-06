

# Scatterplot (Lattice graphics) #
```
## Scatterplot (Lattice graphics).
## Labels are taken from rownames of data.
## Right-click on the plot to identify points.
playwith(xyplot(Income ~ log(Population / Area),
   data = data.frame(state.x77), groups = state.region,
   type = c("p", "smooth"), span = 1, auto.key = TRUE,
   xlab = "Population density, 1974 (log scale)",
   ylab = "Income per capita, 1974"))
```
> ![http://playwith.googlecode.com/files/exampleXY_01.png](http://playwith.googlecode.com/files/exampleXY_01.png)   ![http://playwith.googlecode.com/files/exampleContext_02.png](http://playwith.googlecode.com/files/exampleContext_02.png)

# Scatterplot (base graphics) #
```
## Scatterplot (base graphics); similar.
## Note that label style can be set from a menu item.
urbAss <- USArrests[,c("UrbanPop", "Assault")]
playwith(plot(urbAss, panel.first = lines(lowess(urbAss)),
   col = "blue", main = "Assault vs urbanisation",
   xlab = "Percent urban population, 1973",
   ylab = "Assault arrests per 100k, 1973"))
```
> ![http://playwith.googlecode.com/files/exampleXYBase_01.png](http://playwith.googlecode.com/files/exampleXYBase_01.png)

# Time series plot (Lattice) #
```
## Time series plot (Lattice).
## Date-time range can be entered directly in "time mode"
## (supports numeric, Date, POSIXct, yearmon and yearqtr).
## Click and drag to zoom in, holding Shift to constrain;
## or use the scrollbar to move along the x-axis.
library(zoo)
playwith(xyplot(sunspots ~ yearmon(time(sunspots)),
                xlim = c(1900, 1930), type = "l"),
         time.mode = TRUE)
```
> ![http://playwith.googlecode.com/files/exampleTs_01.png](http://playwith.googlecode.com/files/exampleTs_01.png)

# Time series plot (base graphics) #
```
## Time series plot (base graphics); similar.
## Custom labels are passed directly to playwith.
tt <- time(treering)
treeyears <- paste(abs(tt) + (tt <= 0),
                  ifelse(tt > 0, "CE", "BCE"))
playwith(plot(treering, xlim = c(1000, 1300)),
   labels = treeyears, time.mode = TRUE)
```
> ![http://playwith.googlecode.com/files/exampleTsBase_01.png](http://playwith.googlecode.com/files/exampleTsBase_01.png)

# Multi-panel Lattice plot #
```
## Multi-panel Lattice plot.
## Need subscripts = TRUE to correctly identify points.
## Scales are "same" so zooming applies to all panels.
## Use the 'Panel' tool to expand a single panel, then use
## the vertical scrollbar to change pages.
Depth <- equal.count(quakes$depth, number = 3, overlap = 0.1)
playwith(xyplot(lat ~ long | Depth, data = quakes,
      subscripts = TRUE, aspect = "iso", pch = ".", cex = 2),
   labels = paste("mag", quakes$mag))
```
> ![http://playwith.googlecode.com/files/examplePanels_02.png](http://playwith.googlecode.com/files/examplePanels_02.png)   ![http://playwith.googlecode.com/files/examplePanelsExpand_01.png](http://playwith.googlecode.com/files/examplePanelsExpand_01.png)

# Spin and brush for a 3D Lattice plot #
```
## Spin and zoom for a 3D Lattice plot.
## Drag on the plot to rotate in 3D (can be confusing).
## Brushing is linked to the previous xyplot (if still open).
## Note, brushing 'cloud' requires a recent version of Lattice.
playwith(cloud(-depth ~ long * lat, quakes, zlab = "altitude"),
   new = TRUE, link.to = playDevCur(), click.mode = "Brush")
```
> ![http://playwith.googlecode.com/files/example3DLink_01.png](http://playwith.googlecode.com/files/example3DLink_01.png)

```
## Set brushed points according to a logical condition.
playSetIDs(value = which(quakes$mag >= 6))
```
> ![http://playwith.googlecode.com/files/example3DLinkSetIDs_01.png](http://playwith.googlecode.com/files/example3DLinkSetIDs_01.png)

# Interactive control of a parameter with a slider #
```
## Interactive control of a parameter with a slider.
xx <- rnorm(50)
playwith(plot(density(xx, bw = bandwidth), panel.last = rug(xx)),
	parameters = list(bandwidth = seq(0.05, 1, by = 0.01)))
```
> ![http://playwith.googlecode.com/files/exampleSlider_01.png](http://playwith.googlecode.com/files/exampleSlider_01.png)

# More parameters #
```
## More parameters (logical, numeric, text).
playwith(stripplot(yield ~ site, data = barley,
    jitter = TRUE, type = c("p", "a"),
    aspect = aspect, groups = barley[[groups]],
    scales = list(abbreviate = abbrev),
    par.settings = list(plot.line = list(col = linecol))),
  parameters = list(abbrev = FALSE, aspect = 0.5,
                    groups = c("none", "year", "variety"),
                    linecol = "red"))
```
> ![http://playwith.googlecode.com/files/exampleParameters_01.png](http://playwith.googlecode.com/files/exampleParameters_01.png)

# Composite plot (base graphics) #
```
## Composite plot (base graphics).
## Adapted from an example in help("legend").
## In this case, the initial plot() call is detected correctly;
## in more complex cases may need e.g. main.function="plot".
## Here we also construct data points and labels manually.
x <- seq(-4*pi, 4*pi, by = pi/24)
pts <- data.frame(x = x, y = c(sin(x), cos(x), tan(x)))
labs <- rep(c("sin", "cos", "tan"), each = length(x))
labs <- paste(labs, round(180 * x / pi) \%\% 360)
playwith( {
   plot(x, sin(x), type = "l", xlim = c(-pi, pi),
       ylim = c(-1.2, 1.8), col = 3, lty = 2)
   points(x, cos(x), pch = 3, col = 4)
   lines(x, tan(x), type = "b", lty = 1, pch = 4, col = 6)
   legend("topright", c("sin", "cos", "tan"), col = c(3,4,6),
       lty = c(2, -1, 1), pch = c(-1, 3, 4),
       merge = TRUE, bg = 'gray90')
}, data.points = pts, labels = labs)
```
> ![http://playwith.googlecode.com/files/exampleCompositeBase_01.png](http://playwith.googlecode.com/files/exampleCompositeBase_01.png)

# A ggplot example #
```
## A ggplot example.
## NOTE: only qplot()-based calls will work.
## Labels are taken from rownames of the data.
library(ggplot2)
playwith(qplot(qsec, wt, data = mtcars) + stat_smooth())
```
> ![http://playwith.googlecode.com/files/exampleGgplot_01.png](http://playwith.googlecode.com/files/exampleGgplot_01.png)

# A minimalist grid plot #
```
## A minimalist grid plot.
## This shows how to get playwith to work with custom plots:
## accept xlim/ylim and pass "viewport" to enable zooming.
myGridPlot <- function(x, y, xlim = NULL, ylim = NULL, ...)
{
   if (is.null(xlim)) xlim <- extendrange(x)
   if (is.null(ylim)) ylim <- extendrange(y)
   grid.newpage()
   pushViewport(plotViewport())
   grid.rect()
   pushViewport(viewport(xscale = xlim, yscale = ylim,
      name = "theData"))
   grid.points(x, y, ...)
   grid.xaxis()
   grid.yaxis()
   upViewport(0)
}
playwith(myGridPlot(1:10, 11:20, pch = 17), viewport = "theData")
```
> ![http://playwith.googlecode.com/files/exampleGrid_01.png](http://playwith.googlecode.com/files/exampleGrid_01.png)

# Presenting the window as a modal dialog box #
```
## Presenting the window as a modal dialog box.
## When the window is closed, ask user to confirm.
confirmClose <- function(playState) {
	if (gconfirm("Close window and report IDs?",
                     parent = playState$win)) {
		cat("Indices of identified data points:\n")
		print(playGetIDs(playState))
		return(FALSE) ## close
	} else TRUE ## don't close
}
xy <- data.frame(x = 1:20, y = rnorm(20),
                 row.names = letters[1:20])
playwith(xyplot(y ~ x, xy, main = "Select points, then close"),
        width = 4, height = 3.5, show.toolbars = FALSE,
        on.close = confirmClose, modal = TRUE,
        click.mode = "Brush")
```
> ![http://playwith.googlecode.com/files/exampleOnClose_01.png](http://playwith.googlecode.com/files/exampleOnClose_01.png)

# Cached objects / multiple devices #
```
## Demonstrate cacheing of objects in local environment.
## By default, only local variables in the plot call are stored.
x_global <- rnorm(100)
doLocalStuff <- function(...) {
   y_local <- rnorm(100)
   angle <- (atan2(y_local, x_global) / (2*pi)) + 0.5
   color <- hsv(h = angle, v = 0.75)
   doRays <- function(x, y, col) {
      segments(0, 0, x, y, col = col)
   }
   playwith(plot(x_global, y_local, pch = 8, col = color,
      panel.first = doRays(x_global, y_local, color)),
   ...)
}
doLocalStuff(title = "locals only") ## eval.args = NA is default
```
> ![http://playwith.googlecode.com/files/exampleEnv_01.png](http://playwith.googlecode.com/files/exampleEnv_01.png)

```
## List objects that have been copied and stored:
## Note: if you rm(x_global) now, redraws will fail.
ls(playDevCur()$env)
```
  * `[1] "color"   "doRays"  "y_local"`

```
## Next: store all data objects (in a new window):
doLocalStuff(title = "all stored", eval.args = TRUE, new = TRUE)
ls(playDevCur()$env)
```
  * `[1] "color"    "doRays"   "x_global" "y_local"`

```
## Now there are two devices open:
str(playDevList())
```
  * `List of 2`
  * ` $ all stored :Classes 'playState', 'environment' length 42 <environment>`
  * ` $ locals only:Classes 'playState', 'environment' length 42 <environment>`

```
playDevCur()
```
  * `<playState: all stored>`

```
playDevOff()
playDevCur()
```
  * `<playState: locals only>`

# see also #

`demo(package = "playwith")`