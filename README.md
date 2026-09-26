# Autonomous Magnetic Chessboard

An autonomous, physical mechatronic chessboard that bridges digital chess engines (Stockfish/Python API) with real-world physical actuation via a hidden 2-axis CoreXY Cartesian gantry and electromagnetic coupling.

---

## 📌 System Architecture & Core Concepts

Instead of relying on robotic arms or visible surface gantries, this project uses an under-board **CoreXY motion mechanism** to manipulate pieces invisibly. The system converts high-level move commands (UCI/SAN notation) into continuous physical trajectories, navigating pieces along square boundaries to prevent collisions with other pieces.

---

## 🛠️ Engineering & Technical Breakdown

### 1. Mechatronics & Kinematics
* **CoreXY Motion Architecture:** Chosen over standard Cartesian $X$-$Y$ gantries to keep motor mass stationary, reducing moving inertia and enabling higher velocity/acceleration trajectories during piece relocations.
* **Actuation System:** Dual NEMA 17 stepper motors driven via high-microstepping drivers for smooth, whisper-quiet step execution beneath the playing deck.
* **Electromagnetic Coupling:** Fast-response 12V DC electromagnet driven via MOSFET switching, tuned to engage neodymium magnets embedded in piece bases through the non-magnetic wood/acrylic surface.

### 2. Path Planning & Collision Avoidance Logic
* **Tile-Border Routing:** Since physical chess pieces block direct diagonal trajectories, the path planner routes moved pieces strictly along the 0.5-square boundary offsets (tile edges).
* **Automated Capture Pipeline:** 
  1. Primary trajectory clears the captured piece to an off-grid "graveyard" zone.
  2. Sub-trajectory routes the attacking piece from its origin to the newly vacated target square.
  3. Homing/Limit switches auto-calibrate origin $(0,0)$ on system boot to prevent accumulated step loss.

### 3. Coordinate Transformation
The software layer translates 8×8 algebraic notation (`a1`–`h8`) into physical millimeter coordinates $(X, Y)$ relative to the gantry origin $(X_0, Y_0)$:

$$X_{\text{target}} = X_0 + (\text{file} \times S)$$

$$Y_{\text{target}} = Y_0 + (\text{rank} \times S)$$

*where $S$ represents the physical square dimension (mm).*

---

## 💻 Tech Stack & Skills Demonstrated

* **Software & Algorithms:** Python 3, Trajectory Generation, Path Interpolation, State Machine Design, Chess Engine Integration (`python-chess`, Stockfish API).
* **Embedded Systems & Firmware:** C/C++, Arduino Framework/PlatformIO, Step/Dir Control, Inter-Process Communication (Serial/UART), MOSFET Signal Control.
* **CAD & Hardware Design:** CoreXY Gantry Kinematics, GT2 Timing Belt Drive Systems, Laser-cut Enclosure Prototyping, Power Distribution (12V/5V DC).

---

## 👥 Credits & Attribution

* **Design Origin:** Hardware baseline and structural concepts adapted from the open-source project by *Max.K* on [Instructables](https://www.instructables.com/Automated-Chessboard/).

---

## 📜 License

* **Hardware & CAD Designs:** Creative Commons Attribution-NonCommercial-ShareAlike (CC BY-NC-SA 4.0).
* **Firmware & Control Software:** MIT License.
