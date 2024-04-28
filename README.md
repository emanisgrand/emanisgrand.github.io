# emanisgrand.github.io

---

# Implementing GOAP

## Introduction

Hey there! Welcome to my dev log where I’m building a slick AI system for a future game project. In this, I’ll be diving deep into some cool AI concepts—Finite State Machines (FSM) and Goal-Oriented Action Planning (GOAP). These concepts are the bread and butter of crafting NPCs that don’t just stand around but actually get things done in a dynamic way.

### What’s an FSM?

An FSM, or Finite State Machine, is all about keeping your game characters in check with specific behaviors or states. Think of it like the different vibes you get throughout the day—chill, alert, or flowing. For a game character, this could be chilling in 'patrol' mode, getting hyped in 'chase' mode when they spot you, or HAM in 'attack' mode when they catch up. The FSM manages these transitions smoothly based on what’s going down in the game.

### And GOAP?

Goal-Oriented Action Planning takes things up a notch. It’s like giving your NPCs a mini-brain to plan their moves based on what they want to achieve. Instead of just reacting, they strategize like a chess player, considering various actions to reach their goals while responding to changes in the game environment. I’m still building toward this concept, but in short; GOAP is the GOAL.

Let’s get into how these concepts come together to create some clever NPC behaviors and set the stage for even more advanced AI down the line.

## Journey from FSM to GOAP: Why Inductive?

Now, why take this step-by-step, inductive approach to morphing a basic FSM into a full-fledged GOAP system? Well, it's like learning a new track on guitar—you don't just jump into complex pieces without mastering the scales. Each state in the FSM acts like a chord, and the transitions are the rhythms that connect them. Building this up gradually lets me understand and control how each part interacts before adding in the next layer of complexity.

### Building the FSM

I kicked things off with a simple FSM setup for my game's enemy AI. At this stage, the AI had two main states: `IDLE` and `CHASING`. Here's how it played out:

- **IDLE**: The default vibe. Here, the enemy chills and patrols a designated area, minding its own business unless things pop off—like spotting the player.
- **CHASING**: Triggered when the enemy spots the player within a certain range. Now, the enemy switches up its mood from chill to no-chill actively pursuing the player.

This setup was my foundation. It’s simple but effective, setting the stage for more nuanced behaviors.

```
enum States {
    IDLE,
    CHASING
}

var current_state = States.IDLE

func _physics_process(delta):
    match current_state:
        States.IDLE:
            patrol()
        States.CHASING:
            chase_player()

```

### From Simple to Complex

Once I had the basic states rolling smoothly, I began integrating more complex transitions based on environmental cues and player actions. This wasn't just about seeing the player anymore; it was about deciding the best course of action in real-time, adapting to changes dynamically—essentially setting the groundwork for the GOAP to come.

![https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExemJpa3poMHRwcTZrcHpsNXAzdG45YWYzZHp0Z3Njb2YzdHUzOGprbSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/ecdWpp1dtRgOOWUM7Q/giphy.gif](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExemJpa3poMHRwcTZrcHpsNXAzdG45YWYzZHp0Z3Njb2YzdHUzOGprbSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/ecdWpp1dtRgOOWUM7Q/giphy.gif)

---

## Dynamic Target Tracking

While in the `CHASING` state, it’s crucial that the NPC not only pursues the player but also faces them to simulate realistic engagement. The `look_at` function is used here to dynamically orient the NPC towards the player:

```
func look_at_smoothly(target_position: Vector3, up_direction: Vector3):
    look_at(target_position, up_direction)

```

This function is called every frame during the chase, ensuring that the NPC is always facing the player, adding to the realism and challenge of the game.

### Enhancing the Transition with Tweens

To smooth out the transitions between states and add a flair to the NPC's movements, I integrated Tween animations:

```
func exit_chasing():
    var tween = create_tween()  # Create a new Tween instance
    tween.tween_property(self, "rotation_degrees:y", rotation_degrees.y + 360, 1.0)
    tween.start()

```

This snippet from `exit_chasing` demonstrates how I use a Tween to add a 360-degree spin when the NPC stops chasing, which not only looks cool but also gives me a clear visual cue that the NPC's state has changed. 

---

## Next

We've laid down a solid foundation with our basic FSM, integrating state transitions and dynamic behaviors that pave the way towards a more advanced AI system. This captures just the beginning of creating a sophisticated GOAP system that can handle complex decision-making scenarios for NPCs in games.

### Recap of Our Progress

From setting up the initial states of `IDLE` and `CHASING` to implementing dynamic target tracking and enhancing visual cues with Tween animations, I've got a light framework that not only functions effectively but also looks good in action. The FSM serves as the groundwork from which we can expand, introducing more nuanced behaviors and responses that are typical of GOAP systems.

### Moving Forward

The next phase of development will focus on layering more complexity into our AI's decision-making processes. Here’s what’s coming up:

- **Expanding State Variety**: Introducing additional states such as `ALERT`, `SEARCH`, and `ATTACK` to provide a richer set of behaviors that NPCs can switch between based on their goals and the player's actions.
- **Integrating Environmental Cues**: Making the AI more responsive to the game environment, such as reacting to time of day, other NPCs, or game data.
- **Developing Cost-Based Decision Making**: Each potential action will have a cost associated with it, and the AI will choose actions based on a cost-benefit analysis, bringing us closer to a true GOAP system.

### Wrapping It Up

As I continue to develop and refine the AI, I'll keep documenting the process to share both the triumphs and hurdles along the way.

Stay tuned for more updates. as we push the boundaries of what this game AI can do.

---
### [Read In The Loop: From Building Blocks to Google Blocks](./in-the-loop.md)
### [LinkedIn](https://www.linkedin.com/in/emanuel-martinez-80118760/)
### [Google Blocks](https://sites.google.com/view/em4ngifs/blocks)
### [Licenses & Certifications](https://sites.google.com/view/em4ngifs/38?authuser=0)

