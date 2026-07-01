# NGC 247 H I Kinematics and Data-Reduction Pipeline

An automated data-reduction and analysis pipeline built from scratch in Python to map and study the distribution, kinematics, and global mass of neutral atomic hydrogen ($H_{\mathrm{I}}$) in the nearby late-type spiral galaxy NGC 247. This project utilizes raw 21-cm radio interferometric data cubes obtained from the VLA-ANGST survey.

Methodology
### Step 1: Getting the Raw Data

I started with raw radio data from the VLA-ANGST telescope. This data comes as a giant **4D "data cube"** (which is just a massive stack of hundreds of 2D images, where each image looks at the galaxy at a slightly different velocity channel).

### Step 2: Cleaning the Noise (The Thresholding)

The raw data is completely buried in electronic and background "static" from the empty sky. If you try to map it immediately, you see nothing but background fuzz.

* I built a pipeline to go through the cube and filter out everything below a certain mathematical limit called **$3\sigma$ (3-sigma)**.
* Later, I made this filter stricter by pushing it to **$3.5\sigma$**. This acted like turning up the contrast knob—it erased all the faint background fuzz and made the brightest "high-flux" areas (the orange peaks hitting around $200\text{ Jy/beam}$) stand out with crisp definition.

### Step 3: Compressing the Cube into 2D Maps (The Moments)

Once the data was clean, I squashed the 3D cube down into three different flat 2D maps using a process called "moment mapping":

1. **Moment 0 (The Gas Map):** Stacks all the channels together to show exactly where the physical clumps of hydrogen gas live. This revealed the spiral arms.
2. **Moment 1 (The Speed Map):** Colors the map based on how fast the gas is moving. It created a classic "spider diagram" showing one side of the galaxy rotating toward us (blue-shifted) and the other spinning away (red-shifted).
3. **Moment 2 (The Turbulence Map):** Measures how chaotic the gas is. Most of the galaxy is calm, but the map showed high turbulence near the center where massive newborn stars and supernovas are shaking up the gas.

### Step 4: Solving the Distance Problem (Hubble's Law vs. Reality)

I used the average speed of the galaxy ($V_{\text{sys}} \approx 151.28\text{ km/s}$) and threw it into **Hubble's Law** to calculate how far away it is. The formula spat out a distance of **$2.16\text{ Mpc}$**.

However, I knew from other advanced telescope measurements that the galaxy is actually **$3.4\text{ Mpc}$** away. The reason my calculation was off is because NGC 247 is right in our cosmic backyard. It is so close to us that its own local movement (its *peculiar velocity* caused by being pulled around by neighboring galaxies in the Sculptor Group) completely overpowers the smooth expansion of the universe. Because of this local gravity contamination, I bypassed my calculation and used the trusted $3.4\text{ Mpc}$ distance for my final mass equations.

### Step 5: Weighing the Galaxy (Robust vs. Natural Weighting)

Finally, I ran the data through two different telescope configurations to see how much hydrogen gas is hidden in the galaxy:

* **Robust Weighting:** Focuses heavily on fine details. It gave me a sharp, clumpy map of the main arms and calculated a mass of **$1.37 \times 10^9\,M_\odot$** (about 1.37 billion Suns).
* **Natural Weighting:** Focuses heavily on faint sensitivity. It blurred out the sharp edges but successfully picked up the faint, invisible gas drifting at the very outer edges of the galaxy. This gave me a much higher, more complete mass measurement of **$2.95 \times 10^9\,M_\odot$** (about 2.95 billion Suns).
## Dependencies
* `numpy`
* `astropy` (for FITS file handling and coordinate systems)
* `matplotlib`
