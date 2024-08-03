
```markdown
# 2D Platformer Game

## Overview
This is a 2D platformer game built using the Godot Engine (version 4.2). The game features a start screen, player movement, camera following, player death and respawn mechanics, item collection with inventory, and basic enemy AI.

## Features
- Start screen with a menu to start the game
- Player movement in 4 directions
- Camera that follows the player
- Player death and respawn mechanics
- Item collection and inventory system
- Basic enemy AI that follows the player

## Project Structure
```
res://
│
├── Scenes/
│   ├── Main.tscn
│   ├── Player.tscn
│   ├── Enemy.tscn
│   ├── Item.tscn
│   └── StartScreen.tscn
│
├── Scripts/
│   ├── Player.gd
│   ├── Enemy.gd
│   ├── Item.gd
│   └── StartScreen.gd
│
└── Assets/
    ├── Sprites/
    └── Sounds/
```

## Getting Started

### Prerequisites
- Godot Engine 4.2

### Installation
1. Clone the repository:
    ```sh
    git clone https://github.com/your-username/2d-platformer-game.git
    ```
2. Open the project in Godot Engine.

### Running the Game
1. Open the Godot Engine.
2. Load the project.
3. Run the project by pressing `F5` or clicking on the play button.

## Scripts

### Start Screen
`StartScreen.gd`
```gdscript
extends Control

func _ready():
    $StartButton.connect("pressed", self, "_on_start_button_pressed")

func _on_start_button_pressed():
    get_tree().change_scene("res://Scenes/Game.tscn")
```

### Player Movement
`Player.gd`
```gdscript
extends KinematicBody2D

const SPEED = 200
var velocity = Vector2.ZERO

func _physics_process(delta):
    velocity = Vector2.ZERO
    if Input.is_action_pressed("ui_right"):
        velocity.x += SPEED
    if Input.is_action_pressed("ui_left"):
        velocity.x -= SPEED
    if Input.is_action_pressed("ui_up"):
        velocity.y -= SPEED
    if Input.is_action_pressed("ui_down"):
        velocity.y += SPEED
    velocity = move_and_slide(velocity)
```

### Camera Follow
`Camera2D`
```gdscript
extends Camera2D

func _ready():
    current = true
```

### Player Death and Respawn
`Player.gd` (extended)
```gdscript
extends KinematicBody2D

signal player_died

const SPEED = 200
var velocity = Vector2.ZERO

func _physics_process(delta):
    velocity = Vector2.ZERO
    if Input.is_action_pressed("ui_right"):
        velocity.x += SPEED
    if Input.is_action_pressed("ui_left"):
        velocity.x -= SPEED
    if Input.is_action_pressed("ui_up"):
        velocity.y -= SPEED
    if Input.is_action_pressed("ui_down"):
        velocity.y += SPEED
    velocity = move_and_slide(velocity)

    if is_on_wall():
        emit_signal("player_died")
```

`Main.tscn`
```gdscript
extends Node2D

func _ready():
    $Player.connect("player_died", self, "_on_Player_died")

func _on_Player_died():
    $Player.position = Vector2(100, 100) # Posição inicial do respawn
```

### Item Collection
`Item.gd`
```gdscript
extends Area2D

signal item_collected

func _ready():
    connect("body_entered", self, "_on_body_entered")

func _on_body_entered(body):
    if body.name == "Player":
        emit_signal("item_collected", self)
        queue_free()
```

`Player.gd` (extended for inventory)
```gdscript
extends KinematicBody2D

var inventory = []

func _on_item_collected(item):
    inventory.append(item)
```

### Enemy AI
`Enemy.gd`
```gdscript
extends KinematicBody2D

var target = null
const SPEED = 100

func _process(delta):
    if target:
        var direction = (target.position - position).normalized()
        move_and_slide(direction * SPEED)
```

`Area2D` for enemy detection
```gdscript
extends Area2D

func _ready():
    connect("body_entered", self, "_on_body_entered")

func _on_body_entered(body):
    if body.name == "Player":
        $Enemy.target = body
```

## Contributing
Contributions are welcome! Please open an issue or submit a pull request for any changes.

## License
This project is licensed under the MIT License.

## Acknowledgments
- [Godot Engine](https://godotengine.org/)
- Community tutorials and resources

---

