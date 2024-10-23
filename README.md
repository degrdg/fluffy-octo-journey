from PIL import Image, ImageDraw, ImageFont
import imageio

# Load the GIF
gif_path = "input.gif"
gif = imageio.mimread(gif_path)

# Define the text you want to add
text = "忠诚"
font_size = 30
font_color = (255, 255, 255)  # White color

# Load a font (you can specify a TrueType font file here)
try:
    font = ImageFont.truetype("arial.ttf", font_size)
except IOError:
    font = ImageFont.load_default()

# Process each frame of the GIF and add the text
frames = []
for frame in gif:
    # Convert the frame to a PIL image
    pil_frame = Image.fromarray(frame)
    
    # Create a drawing context
    draw = ImageDraw.Draw(pil_frame)
    
    # Calculate the position of the text to be at the center
    text_width, text_height = draw.textsize(text, font=font)
    width, height = pil_frame.size
    text_position = ((width - text_width) // 2, (height - text_height) // 2)
    
    # Add the text to the frame
    draw.text(text_position, text, font=font, fill=font_color)
    
    # Convert the PIL image back to an array
    frames.append(imageio.core.util.Array(pil_frame))

# Save the modified GIF with the text overlay
output_gif_path = "output.gif"
imageio.mimsave(output_gif_path, frames, duration=0.1)  # duration controls frame speed

print(f"GIF saved as {output_gif_path}")
