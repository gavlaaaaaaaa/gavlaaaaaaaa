---
layout: post
title: Android - How to track steps and calculate running distance
author: Lewis Gavin
comments: true
tags:
- android
- programming
- java
---

## Intro 

I am currently in the process of uplifting an android application I built in my spare time. Between finishing university and starting work I had a fair amount of free time, which I used to hit the gym in attempt to become Hugh Jackman. I found myself trying numerous apps and most of them just contained too much "stuff" and were really bloated. All I wanted was a simple app to track my exercises and maybe show me a graph of my progress. This inspired me to use the rest of my free time to start my own android app. 

The app I made was called Gymify. It is not available on the app store (yet) but at the time it was a simple list based application (see my previous post on my List implementation here: [ListView Adapters](http://gavlaaaaaaaa.github.io/List-View-Adapters/) where a user could add routines and then work through exercises in a routine and tick them off. 

Once my crossover period between finishing uni and starting work had ended I found that I became too busy and progress on the app slowed. Recently I have tried to get myself back into the habit of doing a little bit of coding in my spare time, even if its just 10 minutes a night. This has allowed me to escape work based problems and also focus on different programming techniques using different technologies than those used at work.

One of the enhancements I made to the app recently was to introduce the capability to store cardio based activities. For this I wanted to include tracking the amount of time, steps and distance per workout. This led me to think about how distance could be tracked if, for example, the user was using a treadmill instead of running outside. GPS tracking wouldn't work well in this situation, as the subject is not really moving geographically at all. This post will explore how with a simple library, step tracking can be added to an application, and then show how I used this information to approximate running distances.

## Tracking Steps using SensorEventListener

You'd think tracking steps with some degree of accuracy would be rather difficult to do, but Google couldn't have made it easier with thei android library *SensorEventListener*. By simply implementing this listener within a class and overriding the two methods **onSensorChanged** and **onAccuracyChanged** you can start tracking steps. 

Start by initialising a SensorManager and a Sensor.

~~~java
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;
import android.app.Activity;

public class StepActivity extends Activity implements SensorEventListener{
	SensorManager sManager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);
	Sensor stepSensor = sManager.getDefaultSensor(Sensor.TYPE_STEP_DETECTOR);

	...

}
~~~

The Sensor is initialised as a *TYPE_STEP_DETECTOR* meaning it will be set up to notice when a step has been taken. The manager will be used to register the sensor as a listener to the activity and deregister it when the activity stops. We can do this by overriding the Activities **onResume()** and **onStop()** functions.

~~~java
	@Override
    protected void onResume() {

        super.onResume();

        sManager.registerListener(this, stepSensor, SensorManager.SENSOR_DELAY_FASTEST);

    }

    @Override
    protected void onStop() {
        super.onStop();
        sManager.unregisterListener(this, stepSensor);
    }

~~~

Now we have initialised the SensorManager and Sensor and have the Sensor registered as a listener within the activity, we now need to implement the **onSensorChanged** function that will be triggered by a *SensorEvent* whenever there is a change to the Sensor we registered, in our case the **TYPE_STEP_DETECTOR**.

~~~java
	private long steps = 0;

	@Override
    public void onSensorChanged(SensorEvent event) {
        Sensor sensor = event.sensor;
        float[] values = event.values;
        int value = -1;

        if (values.length > 0) {
            value = (int) values[0];
        }


        if (sensor.getType() == Sensor.TYPE_STEP_DETECTOR) {
            steps++;
        }
    }
~~~

All I am doing is simply incrementing a variable every time a step is detected. This can then be used however you please for example updating the text within a TextView in real time and displaying it to the user.


## Measuring Distance

Now we have a way of tracking steps, we then want to use that data to **determine how far someone has walked/run.** As I mentioned in the intro, a person may not always be running over geographical distances if they are using a treadmill for example, therefore tracking a physical distance using GPS wouldn't be possible. Instead we can use the **average step length** along with the number of steps taken to give a good estimation of distance.

There are many ways to determine step length: you can measure it yourself, estimate by multiplying your height in centimeters by **0.415 for men** and **0.413 for women** or if you're not overly concerned with accuracy you can use the averages **78cm for men** and **70cm for women**. I found these figures using a quick google search and reading up on the first few pages.

For this example I'm going to use 78cm as the step length, as I am male.

So if we take the number of steps, multiply them by 75 and then divide by 100000 we should arrive at a distance in Kilometers. For example, the other day I went for a leisurely run of 6.6 kilometers. According to Google fit I did this in 8,504 steps - so **(8,504 * 78) / 100000 = 6.633** which is pretty *damn* close! 

~~~java
//function to determine the distance run in kilometers using average step length for men and number of steps
public float getDistanceRun(long steps){
    float distance = (float)(steps*78)/(float)100000;
    return distance;
}
~~~

## Wrap up

Thanks for visiting. Hopefully you found this useful. This was something that was really fun for me to play with and try to figure out. I was then able to build this into my application into an activity that recorded cardio exercises by tracking time spent doing the activity, number of steps, estimated distance and from this you could even guess the calories burnt if you had the users height and weight!





