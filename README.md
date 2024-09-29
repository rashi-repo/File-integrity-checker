# File-integrity-checker
To check the integrity of any file and determine if it is altered or not 
import gradio as gr
import hashlib
def check_file_integrity(file, known_checksum):
  """
  Calculates the SHA256 checksum of the uploaded file and compares it with the provided known checksum.

  Args:
    file: The uploaded file.
    known_checksum: The expected SHA256 checksum.

  Returns:
    A string indicating the file integrity status.
  """

  if not file:
    return "Please upload a file"

  try:
    hasher = hashlib.sha256()
    with open(file.name, 'rb') as f:
      for chunk in iter(lambda: f.read(4096), b''):
        hasher.update(chunk)
    calculated_checksum = hasher.hexdigest()

    if calculated_checksum == known_checksum:
      return "File integrity verified!"
    else:
      return "File integrity compromised!"
  except Exception as e:
    return f"An error occurred: {str(e)}"
iface = gr.Interface(
    fn=check_file_integrity,
    inputs=[
        gr.File(label="Upload File"),
        gr.Textbox(label="Known Checksum (SHA256)", placeholder="e.g., d41d8cd98f00b204e9800998ecf8427e3638433f34b4380000000000")
    ],
    outputs="text",
    title="File Integrity Checker"
)

iface.launch()





