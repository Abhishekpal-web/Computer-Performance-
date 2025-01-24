# Computer-Performance-
# This project analyzes the CPU and memory usage of a system in real-time and visualizes the performance metrics.
# Importing necessary libraries
import psutil
import time
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation

# Step 1: Initialize lists to store performance metrics
cpu_usage = []
memory_usage = []
time_stamps = []

# Step 2: Function to fetch system metrics
def fetch_metrics():
    cpu = psutil.cpu_percent(interval=1)
    memory = psutil.virtual_memory().percent
    return cpu, memory

# Step 3: Update function for real-time plotting
def update(frame):
    # Get current metrics
    cpu, memory = fetch_metrics()

    # Append metrics to lists
    time_stamps.append(time.time() - start_time)
    cpu_usage.append(cpu)
    memory_usage.append(memory)

    # Limit lists to the last 50 points for better visualization
    if len(time_stamps) > 50:
        time_stamps.pop(0)
        cpu_usage.pop(0)
        memory_usage.pop(0)

    # Clear the plot
    plt.cla()

    # Plot CPU and memory usage
    plt.plot(time_stamps, cpu_usage, label='CPU Usage (%)', color='red')
    plt.plot(time_stamps, memory_usage, label='Memory Usage (%)', color='blue')

    # Add labels and legends
    plt.xlabel('Time (s)')
    plt.ylabel('Usage (%)')
    plt.title('Real-Time System Performance')
    plt.legend()
    plt.tight_layout()

# Step 4: Main function to start the analysis
def main():
    global start_time
    start_time = time.time()

    # Set up the real-time plot
    plt.figure(figsize=(10, 6))
    ani = FuncAnimation(plt.gcf(), update, interval=1000)

    # Show the plot
    plt.show()

# Run the project
if __name__ == "__main__":
    main()
