# Train an autopilot with Keras

Now that you're able to drive your car reliably you can use Keras to train a
neural network to drive like you. Here are the steps.

## Collect Data

Make sure you collect good data. 

1. Practice driving around the track a couple times without recording data.
2. When you're confident you can drive 10 laps without mistake press `Start Recording`
3. If you crash or run off the track press Stop Car immediately to stop recording. 
A little bad data won't affect your autopilot. 
4. After you've collected 10-20 laps of good data (5-20k images) you can stop 
your car with `Ctrl-c` in the ssh session for your car.
5. The data you've collected is in the data folder in the most recent tub folder.


## Transfer data from your car to you computer. 

Since the Raspberry Pi is not very powerful we need to transfer the data
to our computer to train. 

In a new terminal session on your comptuer use rsync to copy your cars 
folder. 
```
rsync -r pi@<your_pi_ip_address>:~/d2/data  ~/d2/data
```


## Train a model.
5. In the same terminal you can now run the training script on the latest tub.
```
 python ~/d2/manage.py train --tub <tub folder names comma separated> --model mypilot
```

6. Now you can use rsync again to move your pilot back to your car. 
```
rsync ~/d2/models pi@<your_ip_address>:~/d2/models
```

7. Now you can start your car again and pass it your model to drive.
```
python manage.py drive --model ~/d2/models/mypilot
```

## Training Tips:


1. **Mode & Pilot**: Congratulations on getting it this far. The first thing to note after running the command above, is to look at the options in the Mode & Pilot menu. It can be pretty confusing. So here's what the different options mean:
	
	a. **User** : As you guessed, this is where you are in control of both the steering and throttle control.
	
	b. **Local Angle** : Not too obvious, but this is where the trained model (mypilot from above) controls the steering. The _Local_ refers to the trained model which is locally hosted on the raspberry-pi.
	
	c. **Local Pilot** : This is where the trained model (mypilot) assumes control of both the steering and the throttle. As of now, it's purportedly not very reliable.
	
	d. **Auto Angle** : Same as Local Angle, except for it's relying on a model (pilot) running in a separate server. 
	
	e. **Auto Pilot** : Same as Local Pilot, but as you guessed, a remote model assumes control of both the steering and the throttle.

    Be sure to also check out the **Max Throttle** and **Throttle Mode** options, and play around with a few settings. Can help with training quite a lot. 

2. **Build a Simple Track** : This isn't very well-documented, but the car should (theoretically) be able to train against any kind of track. To start off with, it might not be necessary to build a two-lane track with a striped center-lane. Try with a single lane with no center-line, or just a single strip that makes a circuit! At the least, you'll be able to do an end-to-end testing and verify that the software pipeline is all properly functional. Of course, as the next-step, you'll want to create a more standard track, and compete at a [meetup](https://diyrobocars.com/) nearest to you!

3. **Get help** : Try to get some helping hands from a friend or two. Again, this helps immensely with building the track, because it is harder than it looks to build a two-line track on your own! Also, you can save on resources (and tapes) by using a [ribbon](https://www.amazon.com/gp/product/B01M7ZA20R/ref=oh_aui_detailpage_o02_s00?ie=UTF8&psc=1) instead of tapes. They'll still need a bit of tapes to hold them, but you can reuse them and they can be laid down with a lot less effort (Although the wind, if you're working outside, might make it difficult to lay them down initially).
