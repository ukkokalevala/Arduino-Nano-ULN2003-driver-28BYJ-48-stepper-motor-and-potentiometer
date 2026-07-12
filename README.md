how stepper motors actually work!

The short answer is: Because a stepper motor is not a simple DC motor. It is a precise magnetic machine that needs its coils turned on and off in a very specific "dance" to spin.

If the pins are wrong, the dance steps get out of sync, and the motor just shakes instead of spinning.

Here is the simple, visual breakdown of why that specific pin combination (8, 11, 10, 9) matters.

1. The Motor Has 4 "Magnetic Locks"
Inside your 28BYJ-48 motor, there are 4 coils (A, B, C, and D).

Coil A is connected to IN1 (Your Arduino Pin 8)

Coil B is connected to IN2 (Your Arduino Pin 11)

Coil C is connected to IN3 (Your Arduino Pin 10)

Coil D is connected to IN4 (Your Arduino Pin 9)

To make the motor spin, the Arduino has to turn these coils ON and OFF one after another in a strict sequence.

2. The Magic Sequence (The "Dance")
To spin Forward, the Arduino sends this signal:

Step 1: Coil A ON
Step 2: Coil B ON
Step 3: Coil C ON
Step 4: Coil D ON
(Then repeat)

To spin Backward, it does the exact opposite:

Step 1: Coil D ON
Step 2: Coil C ON
Step 3: Coil B ON
Step 4: Coil A ON

3. Why Your First Try Failed (The "Wrong Shoes" Problem)
When you first wired it as (8, 9, 10, 11), you assumed:

Pin 8 = Coil A, Pin 9 = Coil B, Pin 10 = Coil C, Pin 11 = Coil D.

But because of how the ULN2003 board is physically wired, your actual coils were mismatched. It was like telling the motor:

"Turn on Coil A!"
But instead, the Arduino physically turned on Coil C.

Because the coils were in the wrong order, the motor couldn't finish the "dance." It tried to move forward, but the next magnet was in the wrong spot, so it just snapped back and forth (vibrated).

4. Why (8, 11, 10, 9) Works (The "Fix")
When you tested and found (8, 11, 10, 9), you physically corrected the order so that the Arduino's brain matched the actual wires.

Look at what you did:

Your Code Order	Physical Coil	Result
Pin 8	Coil A	Correct!
Pin 11	Coil B	(Swapped from Pin 9 to Pin 11)
Pin 10	Coil C	Correct!
Pin 9	Coil D	(Swapped from Pin 11 to Pin 9)
By swapping pins 9 and 11, you told the Arduino:

"Hey, when I say 'Turn on Coil B', you actually need to send that signal to Pin 11, not Pin 9."

Now, when the Arduino runs the "dance" sequence (A, B, C, D), the electricity actually reaches the physical coils A, B, C, and D in the correct order. Because the sequence is now correct, the motor spins smoothly instead of vibrating.

5. The Gearbox Multiplies the Problem
Your motor has a 64:1 gearbox inside. That means the tiny magnetic rotor has to spin 64 times just to make the output shaft spin once.

If the coil sequence is even slightly wrong, the tiny rotor stalls on the very first step. Because of the gears, you don't see it "miss a step"; you just see the whole output shaft freeze, shake, or hum.

Analogy Time:
Imagine you are trying to walk up an escalator.

The correct order (8, 11, 10, 9): You step onto the stairs in the right rhythm (Left, Right, Left, Right). You go up smoothly.

The wrong order (8, 9, 10, 11): You try to step onto the stairs with a wrong rhythm (Left, Left, Right, Left). You trip, stumble in place, and vibrate, but you never actually go up.

The Arduino's job is just to clap the rhythm (the 4-step sequence). Your job was to physically connect the wires so that the motor's legs (the coils) match that rhythm perfectly. That is why the combination matters!

