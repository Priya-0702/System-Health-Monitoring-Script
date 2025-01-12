# System-Health-Monitoring-Script
import psutil
import logging
from datetime import datetime

# Configure logging
logging.basicConfig(
    filename="system_health.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

# Thresholds
CPU_THRESHOLD = 80  # in percentage
MEMORY_THRESHOLD = 80  # in percentage
DISK_THRESHOLD = 80  # in percentage


def log_alert(message):
    """Logs an alert message to both console and log file."""
    print(message)
    logging.warning(message)


def check_cpu():
    """Check CPU usage."""
    cpu_usage = psutil.cpu_percent(interval=1)
    if cpu_usage > CPU_THRESHOLD:
        log_alert(f"High CPU usage detected: {cpu_usage}% (Threshold: {CPU_THRESHOLD}%)")
    else:
        logging.info(f"CPU usage is normal: {cpu_usage}%")


def check_memory():
    """Check memory usage."""
    memory = psutil.virtual_memory()
    memory_usage = memory.percent
    if memory_usage > MEMORY_THRESHOLD:
        log_alert(f"High memory usage detected: {memory_usage}% (Threshold: {MEMORY_THRESHOLD}%)")
    else:
        logging.info(f"Memory usage is normal: {memory_usage}%")


def check_disk():
    """Check disk usage."""
    disk = psutil.disk_usage('/')
    disk_usage = disk.percent
    if disk_usage > DISK_THRESHOLD:
        log_alert(f"High disk usage detected: {disk_usage}% (Threshold: {DISK_THRESHOLD}%)")
    else:
        logging.info(f"Disk usage is normal: {disk_usage}%")


def check_processes():
    """Check for a high number of running processes."""
    processes = len(psutil.pids())
    # Set an arbitrary high number of processes as a potential issue
    PROCESS_THRESHOLD = 300
    if processes > PROCESS_THRESHOLD:
        log_alert(f"High number of running processes detected: {processes} (Threshold: {PROCESS_THRESHOLD})")
    else:
        logging.info(f"Number of processes is normal: {processes}")


def main():
    """Main function to monitor system health."""
    logging.info("Starting system health monitoring...")
    try:
        check_cpu()
        check_memory()
        check_disk()
        check_processes()
    except Exception as e:
        logging.error(f"An error occurred during monitoring: {e}")

if __name__ == "__main__":
    main()
