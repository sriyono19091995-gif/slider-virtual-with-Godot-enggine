extends Node2D

@onready var force_value = $Control/Control_FORCE/HSlider_FORCE.value
@onready var displacement_value = $Control/Control2_DISPLACEMENT/HSlider_DISPLACEMENT.value
@onready var cos_value = $Control/Control_COSINUS/HSlider_COSINUS.value
@onready var mass_value = $Control/Control_Massa/HSlider_massa.value
enum GameState {SETUP, READY, MOVING, ENDED}
var current_state = GameState.SETUP


@onready var karakter_sprite = $karakter/CollisionShape2D/AnimatedSprite2D
@onready var garis = $Control/Control_COSINUS/Line2D
@onready var objek_sprite = $karakter/CollisionShape2D/balok
@onready var start_timer = $Timer
@onready var button = $Control/Button
@export var line_length: float = 90
@export var line_color: Color = Color(0.6, 0.3, 0.0) 
@export var line_width: float = 4.0
 
var current_angle: float = 45.0  


var result = 0.0
var velocity = Vector2.ZERO

func _ready():
	reset_game()
	setup_line()
	start_timer.one_shot = true
	start_timer.wait_time = 3.0

func reset_game():
	karakter_sprite.position = Vector2(250, 510)
	garis.position = Vector2(53, 230)
	objek_sprite.position = Vector2(100, 550)
	current_state = GameState.SETUP


	velocity = Vector2.ZERO


func setup_line() -> void:
	garis.width = line_width
	garis.default_color = line_color
	garis.antialiased = true
	
	update_line_points(current_angle)

func _on_h_slider_force_value_changed(value: float):
	force_value = value
	current_state = GameState.SETUP

func _on_h_slider_displacement_value_changed(value: float):
	displacement_value = value
	current_state = GameState.SETUP

func _on_h_slider_cosinus_value_changed(value: float):
	cos_value = value
	current_state = GameState.SETUP
	update_visualization(value)




func update_visualization(angle: float) -> void:
	current_angle = angle
	
	update_line_points(angle)
	


func update_line_points(angle_degrees: float) -> void:
	# Konversi ke radian
	var angle_rad = deg_to_rad(angle_degrees)
	var end_point = Vector2(
		-cos(angle_rad) * line_length,   
		-sin(angle_rad) * line_length
	)
	garis.points = PackedVector2Array([Vector2.ZERO, end_point])
func _on_h_slider_massa_value_changed(value: float):
	mass_value = value
	current_state = GameState.SETUP
	



func _on_button_pressed():
	match current_state:
		GameState.SETUP:
			hitung_dan_siapkan()
			current_state = GameState.READY

			
		GameState.READY:
			mulai_gerakan()
			current_state = GameState.MOVING

			
		GameState.ENDED:
			reset_game()

func hitung_dan_siapkan():
	print("=== PERHITUNGAN ===")
	print("Gaya: ", force_value)
	print("Perpindahan: ", displacement_value)
	print("Cosinus: ", cos_value)
	print("Massa: ", mass_value)

	# Rumus usaha: W = F * s * cosθ
	var usaha = force_value * displacement_value * cos_value
	result = usaha - mass_value   # contoh sederhana
	
	print("Usaha: ", usaha, " | Result: ", result)

func mulai_gerakan():

	velocity = Vector2(result, 0)
	start_timer.start()
	print("GERAKAN DIMULAI dengan kecepatan: ", result)



func _process(delta):
	if current_state == GameState.MOVING:
		karakter_sprite.position += velocity * delta
		garis.position += velocity * delta
		objek_sprite.position += velocity * delta


func _on_timer_timeout():
	velocity = Vector2.ZERO

	button.disabled = false
	button.text = "RESET & ULANGI"
	
	print("GERAKAN SELESAI")
	
	var jarak_tempuh = karakter_sprite.position.x - 100
	$Control/LabelHasil.text = "Jarak: " + str(jarak_tempuh)
	reset_game()
