# Generative Art Theory & Techniques Reference

A comprehensive guide to computational aesthetics, mathematical foundations, and implementation patterns for creating museum-quality generative art.

---

## Part I: Mathematical Foundations

### Noise Functions

The backbone of organic-feeling generative art.

#### Perlin Noise
**Ken Perlin, 1983** - Smooth, continuous pseudo-random gradients

```javascript
// Basic Perlin noise in p5.js
let n = noise(x * 0.01, y * 0.01); // Returns 0-1

// Layered octaves for complexity (fractal Brownian motion)
function fbm(x, y, octaves = 4) {
  let total = 0;
  let frequency = 1;
  let amplitude = 1;
  let maxValue = 0;
  
  for (let i = 0; i < octaves; i++) {
    total += noise(x * frequency, y * frequency) * amplitude;
    maxValue += amplitude;
    amplitude *= 0.5;  // persistence
    frequency *= 2;    // lacunarity
  }
  
  return total / maxValue;
}
```

**Parameters:**
- **Frequency**: Scale of features (lower = larger features)
- **Octaves**: Layers of detail (more = finer detail)
- **Persistence**: Amplitude decay per octave (0.5 typical)
- **Lacunarity**: Frequency multiplier per octave (2.0 typical)

#### Simplex Noise
**Ken Perlin, 2001** - Improved: fewer artifacts, better performance in higher dimensions

#### Worley/Voronoi Noise
**Steven Worley, 1996** - Distance to nearest point creates cellular patterns

```javascript
// Conceptual Worley noise
function worleyNoise(x, y, points) {
  let minDist = Infinity;
  for (let p of points) {
    let d = dist(x, y, p.x, p.y);
    minDist = min(minDist, d);
  }
  return minDist;
}
```

---

### Vector Fields

Mathematical functions that assign vectors to points in space.

#### Flow Field Construction

```javascript
// Noise-based flow field
function getFlowVector(x, y) {
  let angle = noise(x * 0.005, y * 0.005) * TWO_PI * 2;
  return createVector(cos(angle), sin(angle));
}

// Mathematical flow field (curl of a potential)
function curlField(x, y) {
  let eps = 0.0001;
  // Potential function
  let n = (x, y) => sin(x * 0.1) * cos(y * 0.1);
  
  // Partial derivatives
  let dx = (n(x, y + eps) - n(x, y - eps)) / (2 * eps);
  let dy = -(n(x + eps, y) - n(x - eps, y)) / (2 * eps);
  
  return createVector(dx, dy);
}
```

#### Classic Flow Field Patterns

| Pattern | Formula | Character |
|---------|---------|-----------|
| Circular | `atan2(y-cy, x-cx)` | Spiral around center |
| Sink/Source | Point toward/away from center | Attraction/repulsion |
| Saddle | `atan2(y, x) + atan2(-y, x)` | Four-way flow |
| Turbulent | Multi-octave noise | Organic chaos |

---

### Particle Systems

Agents moving through space according to rules.

#### Particle Lifecycle

```javascript
class Particle {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.lifespan = 255;
    this.trail = [];
  }
  
  applyForce(force) {
    this.acc.add(force);
  }
  
  follow(flowField) {
    let x = floor(this.pos.x / flowField.resolution);
    let y = floor(this.pos.y / flowField.resolution);
    let force = flowField.lookup(x, y);
    this.applyForce(force);
  }
  
  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 2;
    this.trail.push(this.pos.copy());
    if (this.trail.length > 50) this.trail.shift();
  }
  
  display() {
    stroke(255, this.lifespan);
    noFill();
    beginShape();
    for (let p of this.trail) {
      vertex(p.x, p.y);
    }
    endShape();
  }
  
  isDead() {
    return this.lifespan < 0;
  }
  
  edges() {
    // Wrap, bounce, or respawn at edges
  }
}
```

#### Particle Behaviors

| Behavior | Implementation | Visual Effect |
|----------|----------------|---------------|
| Flocking | Boids algorithm (separation, alignment, cohesion) | Organic swarm motion |
| Attraction | Force toward point(s) | Gravitational clustering |
| Repulsion | Force away from point(s) | Explosive dispersion |
| Following | Velocity from flow field | River-like streams |
| Collision | Distance check + response | Dense packing |

---

### Geometric Algorithms

#### Voronoi Tessellation

Partition space by nearest point.

```javascript
// Using a library or manual implementation
// Each cell contains all points closer to its seed than any other seed
```

**Applications:**
- Organic cell structures
- Cracked earth patterns
- Territory visualization

#### Delaunay Triangulation

Connect points such that no point is inside any triangle's circumcircle.

**Applications:**
- Mesh generation
- Natural terrain
- Point cloud connectivity

#### Circle Packing

Fill space with non-overlapping circles.

```javascript
// Relaxation approach
function packCircles(circles, iterations) {
  for (let i = 0; i < iterations; i++) {
    for (let a of circles) {
      for (let b of circles) {
        if (a !== b) {
          let d = dist(a.x, a.y, b.x, b.y);
          let minDist = a.r + b.r;
          if (d < minDist) {
            let angle = atan2(b.y - a.y, b.x - a.x);
            let pushDist = (minDist - d) / 2;
            a.x -= cos(angle) * pushDist;
            a.y -= sin(angle) * pushDist;
            b.x += cos(angle) * pushDist;
            b.y += sin(angle) * pushDist;
          }
        }
      }
    }
  }
}
```

#### L-Systems (Lindenmayer Systems)

Grammar-based recursive structures.

```javascript
// Example: Koch curve
let axiom = "F";
let rules = { "F": "F+F-F-F+F" };
let angle = 90;

function generate(str) {
  let next = "";
  for (let c of str) {
    next += rules[c] || c;
  }
  return next;
}

function interpret(str) {
  for (let c of str) {
    switch(c) {
      case 'F': line(0, 0, len, 0); translate(len, 0); break;
      case '+': rotate(angle); break;
      case '-': rotate(-angle); break;
      case '[': push(); break;
      case ']': pop(); break;
    }
  }
}
```

**Classic L-Systems:**
| Name | Axiom | Rule | Angle |
|------|-------|------|-------|
| Koch | F | F+F-F-F+F | 90° |
| Sierpinski | F-G-G | F→F-G+F+G-F, G→GG | 120° |
| Plant | X | X→F+[[X]-X]-F[-FX]+X, F→FF | 25° |

---

## Part II: Color Theory for Generative Art

### Color Spaces

#### HSB (Hue, Saturation, Brightness)
Best for programmatic color manipulation.

```javascript
colorMode(HSB, 360, 100, 100);

// Hue cycling
let h = (frameCount * 0.5) % 360;

// Complementary
let complement = (h + 180) % 360;

// Analogous palette
let colors = [h, (h + 30) % 360, (h - 30 + 360) % 360];

// Triadic palette
let triadic = [h, (h + 120) % 360, (h + 240) % 360];
```

#### Color Harmony Algorithms

```javascript
// Golden ratio color spacing (pleasing distribution)
function goldenColors(count, startHue = random(360)) {
  let colors = [];
  let goldenRatio = 0.618033988749895;
  let h = startHue;
  for (let i = 0; i < count; i++) {
    colors.push(color(h, 70, 90));
    h = (h + goldenRatio * 360) % 360;
  }
  return colors;
}
```

### Color Mapping Techniques

| Technique | Use Case | Effect |
|-----------|----------|--------|
| Velocity → Hue | Particle systems | Energy visualization |
| Position → Color | Spatial gradients | Location encoding |
| Age → Alpha | Trails | Temporal fading |
| Density → Saturation | Accumulation | Heat map feel |
| Noise → Palette Index | Organic variation | Natural color shifts |

---

## Part III: Composition Principles

### Dynamic Balance

Unlike static design, generative art creates balance through:
- **Density distribution**: Particles naturally cluster and disperse
- **Temporal averaging**: Motion blurs create visual equilibrium
- **Statistical balance**: Random processes tend toward uniformity over time

### Emergence

The magic of generative art: simple rules → complex results.

**Levels of emergence:**
1. **First-order**: Direct visual output of algorithm
2. **Second-order**: Patterns from interaction (flocking, collision)
3. **Third-order**: Meaning from accumulation over time

### Scale and Detail

```javascript
// Multi-scale approach
function draw() {
  // Background: large, slow noise
  background(noise(frameCount * 0.001) * 50);
  
  // Mid-ground: medium features
  drawMidgroundElements();
  
  // Foreground: fine detail, fast motion
  drawParticles();
}
```

---

## Part IV: Advanced Techniques

### Recursive Subdivision

```javascript
function subdivide(x, y, w, h, depth) {
  if (depth === 0 || random() < 0.1) {
    rect(x, y, w, h);
    return;
  }
  
  let split = random(0.3, 0.7);
  if (random() < 0.5) {
    subdivide(x, y, w * split, h, depth - 1);
    subdivide(x + w * split, y, w * (1 - split), h, depth - 1);
  } else {
    subdivide(x, y, w, h * split, depth - 1);
    subdivide(x, y + h * split, w, h * (1 - split), depth - 1);
  }
}
```

### Differential Growth

Simulate organic growth patterns.

```javascript
// Conceptual approach
class GrowingLine {
  constructor() {
    this.nodes = [];  // Points along line
  }
  
  grow() {
    // Insert new nodes between existing ones
    // Apply forces: attraction to neighbors, repulsion from others
    // Constrain to boundaries
  }
}
```

### Reaction-Diffusion

Gray-Scott model for organic patterns.

```javascript
// Two-chemical system
// A + 2B → 3B (autocatalysis)
// B → P (removal)
// Creates spots, stripes, maze patterns
```

### Attractor Systems

Strange attractors create mesmerizing patterns.

```javascript
// Lorenz attractor
function lorenz(x, y, z) {
  let sigma = 10, rho = 28, beta = 8/3;
  let dx = sigma * (y - x);
  let dy = x * (rho - z) - y;
  let dz = x * y - beta * z;
  return { dx, dy, dz };
}
```

---

## Part V: Performance Optimization

### Frame Rate Maintenance

```javascript
// Batch rendering
function draw() {
  for (let i = 0; i < 100; i++) {  // Multiple steps per frame
    updateParticles();
  }
  renderOnce();
}

// Spatial hashing for collision detection
class SpatialHash {
  constructor(cellSize) {
    this.cellSize = cellSize;
    this.cells = {};
  }
  // O(n) instead of O(n²) neighbor checking
}

// Off-screen buffer for trails
let trailBuffer;
function setup() {
  trailBuffer = createGraphics(width, height);
}
function draw() {
  trailBuffer.background(0, 5);  // Fade existing
  drawNewTrails(trailBuffer);
  image(trailBuffer, 0, 0);
}
```

### Memory Management

```javascript
// Object pooling
let particlePool = [];
function getParticle() {
  return particlePool.pop() || new Particle();
}
function releaseParticle(p) {
  p.reset();
  particlePool.push(p);
}

// Trail length limits
if (trail.length > MAX_TRAIL) trail.shift();
```

---

## Part VI: Seeded Reproducibility

### Seed Management

```javascript
let seed;

function setup() {
  seed = params.seed || floor(random(1000000));
  randomSeed(seed);
  noiseSeed(seed);
  // Now all random/noise calls are reproducible
}

// Same seed = identical output
// Different seeds = unique variations
// Enables: sharing, printing, exploration
```

### Parameter Spaces

Design parameters that create interesting variation:

| Parameter Type | Good Range | Effect |
|----------------|------------|--------|
| Particle count | 100-10000 | Density |
| Noise scale | 0.001-0.05 | Feature size |
| Speed | 0.5-5.0 | Energy level |
| Trail length | 10-500 | History visualization |
| Force strength | 0.01-0.5 | Behavioral intensity |

---

## Part VII: Gallery-Quality Output

### Resolution Independence

```javascript
let scaleFactor = 4;  // For high-res export

function setup() {
  createCanvas(1200 * scaleFactor, 1200 * scaleFactor);
  // Scale all coordinates and sizes
}

function exportHighRes() {
  saveCanvas('artwork', 'png');
}
```

### Anti-Aliasing

```javascript
// Smooth lines
smooth();

// Custom anti-aliasing for particles
function drawSmoothCircle(x, y, r) {
  noStroke();
  for (let i = r; i > 0; i -= 0.5) {
    let alpha = map(i, 0, r, 255, 0);
    fill(currentColor, alpha);
    ellipse(x, y, i * 2);
  }
}
```

### Export Formats

| Format | Use Case | Notes |
|--------|----------|-------|
| PNG | Web, screen | Lossless, transparency |
| TIFF | Print | High quality, large files |
| SVG | Vector output | Scalable, editability |
| PDF | Print | Vector, multi-page |

---

*This reference embodies decades of generative art practice. Use these techniques as building blocks, not recipes—true mastery comes from combining them in novel ways that express your unique algorithmic vision.*
