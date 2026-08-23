# Bestiolino-full-power-management
PMS for portable console 
New Version 2.0 , completely redesigned:

    TPS62816 --> the heart of the "Nano" 6A maximum for cpu/gpu core
    Rt9193 --> I’ve taken a step back from my previous approach—not because of the LDO issue often discussed regarding the PS2, but mainly due to the soldering difficulty caused by its tiny size, and because I wanted to propose a project that is feasible for everyone. Furthermore, since both 1.5V and 1.8V versions are available, it can easily be swapped out if any instability arises.You just need to twist the cables supplying power to the PS2.
    TPS628501 --> There are also various variables available (see datasheet) in this setup it will be used for 2.5v - 1A Max
    TPS63802 --> Here is the "champion" IC , already extensively covered on my 79001, a hybrid converter capable of changing its internal operation to constantly deliver 3.5v 2A Max
    TPS61023DRLR--> 5V - 3A available - The other one had MOSFETs that were too large and didn't handle the op-amp's variable load well at maximum volume—plus, the output filtering was reinforced.
    Sw6106 --> last but not least, the heart of it all is a power bank IC that in addition to "giving us" a second 5V rail so as to separate for example PS2 / audio / LCD as we prefer, manages the cell charging and much more. I added the CC1 and CC2 lines, so it can easily support fast charging from 5V up to 12V, delivering up to 15W.

Let's look at the pros and cons:
Pro:

    Extremely small and versatile (slightly smaller than the v1)
    Very efficient in terms of both energy and heat (it practically only emits heat while charging the battery).
    It doesn't have the 3.3V-3.5V issue—which, from what I gather, affects not only the PMS1 but also the PMS2—so it utilizes 99% of the battery.
    No ICs to program or anything else; a simple resistor (or lack thereof) can set the output battery voltage (from 4.2V to 4.4V).
    all the appropriate protections (however, it is recommended to use the dedicated protection circuit for the battery output)
    On-board NTC for additional protection, plus all the various monitoring and management LEDs.
    I chose the SW6106—despite it being an older IC that doesn't use modern technologies—because it lacks a charge level indicator (fuel gauge). What does this mean in simple terms? Basically, you can disconnect and reconnect your battery with peace of mind without destabilizing the system's charge percentage reading. You connect the battery, the IC reads the voltage and assigns a percentage—easy, fast, and immediate. With ICs that have a fuel gauge, you are forced to drain the battery to 0% and recharge it; otherwise, the reading will be incorrect, leading to major issues (though this depends on the specific chip).
    All the ICs belong to the same family and have easily configurable output voltages (excluding the LDO and SW, of course).

Cons:

    Given its compact size, I realize that even though the components are "easy" to solder, a novice might face significant difficulties, as many of them are stacked on top of one another.
    I have run several tests so far (I haven't thoroughly analyzed the original circuit), and it appears that charging is limited to 15 W (at least at 9 V; the figure drops at 5 V) compared to the SW6106 reference PCB.
    It lacks a touch button for activation; I list this as a downside, though my goal was to have zero standby power consumption.
    Operation is slightly "unusual" – see below.

It has currently been tested on the 90004 console, though not long enough to say with 100% certainty that it is problem-free; I mention this merely as a precaution because I like to be transparent, but I don't foresee any major issues that would prevent its use.
The voltages are standard, except that:

    EE Power is set to 0.975v . So it's best to use thick cables that are as short as possible; it works fine on my 79001 and on this 90004 I'm testing.
    The RAM voltage is set to 1.5V, but if issues arise, simply replace the IC with the 1.8V model; it draws input power from the 2.5V rail to optimize power consumption.
    The remaining rails are identical to the originals and meet standard specifications.

Operating principle:
The switch (SW1 and SW2) is triggered via a resistor connected between its "key" pin and ground to "wake it up"; this way, as soon as the circuit closes, it immediately begins supplying power (in this case, the "5vlcd" line). This line serves both as a secondary power source and as an enable signal for downstream regulators; consequently, using it is strictly mandatory—otherwise, without a load, the switch will cut off the voltage, rendering the circuit inactive (though a bypass comes later, lol).
When charging the console while it is powered on, the (5V LCD) output drops for about 500ms; the D1, R1, and C3 circuit handles this to prevent the main regulators from shutting down. Should you encounter issues here, simply increasing the capacitance of C3 would solve it, though I do not believe that will be necessary, as I have conducted sufficient testing.
Therefore, the 5VLCD rail should not be used to power critical components—such as USB devices or others—that would cause the system to crash in the event of a momentary power interruption.
For the LCD and Amp, you can even connect to the PS2's 5V line—just avoid USB or anything else that might cause the game to freeze.
As above, but if the device is charging in fast-charge mode, how can it output 5V? Fortunately, it immediately switches the input to 5V regardless of the source voltage, so there are no issues with the output rail (5VLcd).
The battery's positive terminal is permanently connected to the regulators' positive terminals, so keep this detail in mind. Another point to note is that the circuit features two separate GND connections. Based on the operating principle described above, the system will function once connected. Since this is a power connection, ensure you use cables and a switch capable of handling the required current.

2026-08-23_00h53_17.webp2026-08-23_00h54_11.webp
