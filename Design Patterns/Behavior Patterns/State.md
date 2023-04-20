# State

## Simple Explanation

The State design pattern is a behavioral design pattern that allows an object to alter its behavior when its internal state changes. It promotes loose coupling and encapsulates state-specific logic into separate classes, making it easier to extend and maintain the code.

## Deep Explanation

The State pattern involes three main components:

1. Context - the class that holds a reference to a state object and delegates state-specific behavior to it. The context can change its state object at runtime, altering its behavior accordingly.

2. State - an interface or abstract class that defines a common interface for all concrete state classes. It declares methods for handling state-specific behavior.

3. Concrete States - classes implementing the State interface, encapsulating the state-specific behavior for each possible state of the context.

By implementing the State pattern, you can organize state-specific logic into separate classes, making it easier to extend the code with new states and reducing the complexity of the context class.

## Examples

Let's imagine we have a simple audio player that can be in three states: Playing, Paused, and Stopped.

1. Create the State interface:

```C#
public interface IAudioPlayerState
{
    void Play(AudioPlayer player);
    void Pause(AudioPlayer player);
    void Stop(AudioPlayer player);
}
```

2. Implement Concrete State classes:

```C#
public class PlayingState : IAudioPlayerState
{
    public void Play(AudioPlayer player)
    {
        Console.WriteLine("Already playing.");
    }
    
    public void Pause(AudioPlayer player)
    {
        Console.WriteLine("Pausing the playback.");
        player.SetState(new PausedState());
    }

    public void Stop(AudioPlayer player)
    {
        Console.WriteLine("Stopping the playback.");
        player.SetState(new StoppedState());
    }
}

public class PausedState : IAudioPlayerState
{
   public void Play(AudioPlayer player)
    {
        Console.WriteLine("Resuming the playback.");
        player.SetState(new PlayingState());
    }
    
    public void Pause(AudioPlayer player)
    {
        Console.WriteLine("Already paused.");
    }

    public void Stop(AudioPlayer player)
    {
        Console.WriteLine("Stopping the playback.");
        player.SetState(new StoppedState());
    } 
}

public class StoppedState : IAudioPlayerState
{
    public void Play(AudioPlayer player)
    {
        Console.WriteLine("Starting the playback.");
        player.SetState(new PlayingState());
    }
    
    public void Pause(AudioPlayer player)
    {
        Console.WriteLine("Can't pause. The player is stopped.");
    }

    public void Stop(AudioPlayer player)
    {
        Console.WriteLine("Already stopped.");
    }
}
```

3. Implement the Context class:

```C#
public class AudioPlayer
{
    private IAudioPlayerState _state;

    public AudioPlayer()
    {
        _state = new StoppedState();
    }

    public void SetState(IAudioPlayerState state)
    {
        _state = state;
    }

    public void Play()
    {
        _state.Play(this);
    }

    public void Pause()
    {
        _state.Pause(this);
    }

    public void Stop()
    {
        _state.Stop(this);
    }
}
```

4. Use the Context and State objects:

```C#
class Program
{
    static void Main(string[] args)
    {
        var player = new AudioPlayer();

        player.Play();
        player.Pause();
        player.Stop();
        player.Play();

        /* Output:
            Starting the playback.
            Pausing the playback.
            Stopping the playback.
            Starting the playback.
        */
    }
}
```

In this example, `AudioPlayer` is the Context, and `PlayingState`, `PausedState`, and `StoppedState` are Concrete State classes. The `AudioPlayer` delegates state-specific behavior to the state objects, which allows it to change its behavior dynamically based on its current state.

By using the State pattern, you can organize state-specific logic into separate classes, making it easier to extend the code with new states and reducing the complexity of the context class. This promotes loose coupling and improves code maintainability.

When the `AudioPlayer`'s `Play`, `Pause`, or `Stop` methods are called, it delegates the behavior to the current state object. The state object handles the request and can also change the state of the `AudioPlayer` by calling its `SetState` method. This allows the `AudioPlayer` to switch between different behaviors depending on its current state, without having to manage the state transitions and related logic directly within the `AudioPlayer` class.