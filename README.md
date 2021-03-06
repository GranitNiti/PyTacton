# PyTact:
This is a lightweight Python Module for Haptic Feedback suitable for easy prototyping user studies that involve haptic stimulation. It provides easy means of creating complex patterns, stimulating and visualising them. The support for devices can be achieved through minimal code (see belove). If you add support for a device you use, please push the changes so that others can benefit from your code.

###### Code sample for creating an stimulation engine, creating a complex pattern and stimulating it

```python
#step 1 - creating a visualiser
vis = LinePatternVisualiser(no_actuators=3)

#step 2 - creating a stimulation engine - hardware device
cfg = VibroStimulationEngineConfig(port="/dev/cu.usbFA1")
eng = VibroStimulationEngine(visualiser=vis, config=cfg)

#step 3 - creating a pattern composed of three motors:
t1 = SingleTactonVibro(duration=0.5, channel=1)
t2 = SingleTactonVibro(duration=0.5, channel=2)
t3 = SingleTactonVibro(duration=0.5, channel=3)
tacton = PatternTacton(gap=0.2, tactons=[t1, t3, t2])

#step 4 - stimulating the tacton
eng.stimulate_pattern(tacton)
 ```

-------------------------------------------------

###### To add support for a new device the minimal code should be provided:

```python
Class MyStimulationEngine(StimulationEngine):
    def _connect(self):
        pass

    def _disconnect(self):
        pass

    #if intenstiy is provided then it overrides the intnsity of tacton, otherwise the intensity of tacton is used
    def _start_stimulation_tacton(self, tacton, intensity=None):
        pass

    '''
    if intensties are provided then it overrides the intnsity of tacton, otherwise the intensities of tacton are used
    This is optional, if it is not implemented then _start_stimulation_tacton is used for each tacton
    '''
    def _start_stimulation_tactons(self, tactons, intensities=None):
        pass

    def _stop_stimulation(self, tacton):
        pass
 ```


-------------------------------------------------

###### To create a custom visualisation:

```python
# list of pairs (x,y) that represent coordinates of actuators
positions = []
# the radius of each actuator that is shown on the screen
actuator_radius=25

#create visualiser
visualiser = GenericPatternVisualiser(positions, actuator_radius)

#pass it to engine and then framework takes care of the rest
eng = VibroStimulationEngine(visualiser=vis, config=cfg)
```
