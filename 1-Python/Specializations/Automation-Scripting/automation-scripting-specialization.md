# Python Automation & Scripting - Practical Efficiency Path

Turn repetitive tasks into automated solutions. This isn't about showing off with complex code - it's about solving real problems and saving time through smart automation.

## Reality Check First

**What Automation Actually Is:**
- 90% understanding the problem and edge cases
- 8% writing the actual automation code
- 2% the "cool" Python tricks you see in tutorials

**What You'll Actually Build:**
- File processing and organization systems
- Data extraction and transformation tools
- System administration utilities
- Web scraping and API integration scripts
- Report generation and email automation
- Workflow automation tools

**What You Won't Become:**
- A software architect (different skill set)
- Someone who automates everything (some things shouldn't be automated)
- A replacement for proper system design

## Prerequisites

**From Python Fundamentals:**
- Comfortable with file operations and error handling
- Understanding of loops, functions, and data structures
- Experience with modules and packages

**Additional Skills:**
- **Command Line Basics**: Navigating directories, running scripts
- **File System Understanding**: Paths, permissions, file types
- **Basic Regex**: Pattern matching for text processing
- **Problem Decomposition**: Breaking complex tasks into steps

## Phase 1: File and System Operations

### File Management Automation

**Why This Matters**: Most automation starts with organizing or processing files

```python
import os
import shutil
from pathlib import Path
import mimetypes
from datetime import datetime
import hashlib

class FileOrganizer:
    def __init__(self, source_dir, organized_dir):
        self.source_dir = Path(source_dir)
        self.organized_dir = Path(organized_dir)
        self.organized_dir.mkdir(exist_ok=True)
        
        # File type mappings
        self.type_mappings = {
            'images': ['.jpg', '.jpeg', '.png', '.gif', '.bmp', '.svg', '.webp'],
            'documents': ['.pdf', '.doc', '.docx', '.txt', '.rtf', '.odt'],
            'spreadsheets': ['.xls', '.xlsx', '.csv', '.ods'],
            'presentations': ['.ppt', '.pptx', '.odp'],
            'videos': ['.mp4', '.avi', '.mkv', '.mov', '.wmv', '.flv', '.webm'],
            'audio': ['.mp3', '.wav', '.flac', '.aac', '.ogg', '.wma'],
            'archives': ['.zip', '.rar', '.7z', '.tar', '.gz', '.bz2'],
            'code': ['.py', '.js', '.html', '.css', '.java', '.cpp', '.c', '.php']
        }
    
    def get_file_category(self, file_path):
        """Determine file category based on extension"""
        extension = file_path.suffix.lower()
        
        for category, extensions in self.type_mappings.items():
            if extension in extensions:
                return category
        
        return 'others'
    
    def get_file_hash(self, file_path):
        """Generate MD5 hash for duplicate detection"""
        hash_md5 = hashlib.md5()
        try:
            with open(file_path, "rb") as f:
                for chunk in iter(lambda: f.read(4096), b""):
                    hash_md5.update(chunk)
            return hash_md5.hexdigest()
        except Exception as e:
            print(f"Error hashing {file_path}: {e}")
            return None
    
    def organize_by_type(self):
        """Organize files by type into subdirectories"""
        moved_files = []
        errors = []
        
        for file_path in self.source_dir.rglob('*'):
            if file_path.is_file():
                try:
                    category = self.get_file_category(file_path)
                    category_dir = self.organized_dir / category
                    category_dir.mkdir(exist_ok=True)
                    
                    # Handle name conflicts
                    destination = category_dir / file_path.name
                    counter = 1
                    while destination.exists():
                        stem = file_path.stem
                        suffix = file_path.suffix
                        destination = category_dir / f"{stem}_{counter}{suffix}"
                        counter += 1
                    
                    shutil.move(str(file_path), str(destination))
                    moved_files.append((str(file_path), str(destination)))
                    
                except Exception as e:
                    errors.append(f"Error moving {file_path}: {e}")
        
        return {
            'moved_files': moved_files,
            'errors': errors,
            'total_moved': len(moved_files)
        }
    
    def organize_by_date(self):
        """Organize files by creation date"""
        moved_files = []
        errors = []
        
        for file_path in self.source_dir.rglob('*'):
            if file_path.is_file():
                try:
                    # Get file creation time
                    creation_time = datetime.fromtimestamp(file_path.stat().st_ctime)
                    year_month = creation_time.strftime('%Y-%m')
                    
                    # Create year-month directory
                    date_dir = self.organized_dir / year_month
                    date_dir.mkdir(exist_ok=True)
                    
                    # Move file
                    destination = date_dir / file_path.name
                    counter = 1
                    while destination.exists():
                        stem = file_path.stem
                        suffix = file_path.suffix
                        destination = date_dir / f"{stem}_{counter}{suffix}"
                        counter += 1
                    
                    shutil.move(str(file_path), str(destination))
                    moved_files.append((str(file_path), str(destination)))
                    
                except Exception as e:
                    errors.append(f"Error moving {file_path}: {e}")
        
        return {
            'moved_files': moved_files,
            'errors': errors,
            'total_moved': len(moved_files)
        }
    
    def find_duplicates(self):
        """Find duplicate files based on content hash"""
        file_hashes = {}
        duplicates = []
        
        for file_path in self.source_dir.rglob('*'):
            if file_path.is_file():
                file_hash = self.get_file_hash(file_path)
                if file_hash:
                    if file_hash in file_hashes:
                        duplicates.append({
                            'original': file_hashes[file_hash],
                            'duplicate': str(file_path),
                            'size': file_path.stat().st_size
                        })
                    else:
                        file_hashes[file_hash] = str(file_path)
        
        return duplicates
    
    def cleanup_empty_dirs(self):
        """Remove empty directories"""
        removed_dirs = []
        
        for dir_path in sorted(self.source_dir.rglob('*'), reverse=True):
            if dir_path.is_dir():
                try:
                    if not any(dir_path.iterdir()):
                        dir_path.rmdir()
                        removed_dirs.append(str(dir_path))
                except OSError:
                    pass  # Directory not empty or permission error
        
        return removed_dirs

# Usage example
def organize_downloads():
    """Organize a typical Downloads folder"""
    downloads_path = Path.home() / "Downloads"
    organized_path = Path.home() / "Organized_Downloads"
    
    organizer = FileOrganizer(downloads_path, organized_path)
    
    print("Finding duplicates...")
    duplicates = organizer.find_duplicates()
    if duplicates:
        print(f"Found {len(duplicates)} duplicate files:")
        for dup in duplicates[:5]:  # Show first 5
            print(f"  {dup['duplicate']} (duplicate of {dup['original']})")
    
    print("\nOrganizing by file type...")
    result = organizer.organize_by_type()
    print(f"Moved {result['total_moved']} files")
    
    if result['errors']:
        print(f"Encountered {len(result['errors'])} errors:")
        for error in result['errors'][:3]:  # Show first 3
            print(f"  {error}")

# Run the organizer
# organize_downloads()
```

### System Information and Monitoring

**Real-World Application**: Understanding and monitoring your system

```python
import psutil
import platform
import socket
import subprocess
import json
from datetime import datetime, timedelta

class SystemMonitor:
    def __init__(self):
        self.hostname = socket.gethostname()
        self.platform_info = platform.uname()
    
    def get_system_info(self):
        """Get comprehensive system information"""
        return {
            'hostname': self.hostname,
            'platform': {
                'system': self.platform_info.system,
                'node': self.platform_info.node,
                'release': self.platform_info.release,
                'version': self.platform_info.version,
                'machine': self.platform_info.machine,
                'processor': self.platform_info.processor
            },
            'python_version': platform.python_version(),
            'timestamp': datetime.now().isoformat()
        }
    
    def get_cpu_info(self):
        """Get CPU usage and information"""
        return {
            'cpu_count': psutil.cpu_count(),
            'cpu_count_logical': psutil.cpu_count(logical=True),
            'cpu_percent': psutil.cpu_percent(interval=1),
            'cpu_percent_per_core': psutil.cpu_percent(interval=1, percpu=True),
            'cpu_freq': psutil.cpu_freq()._asdict() if psutil.cpu_freq() else None,
            'load_average': psutil.getloadavg() if hasattr(psutil, 'getloadavg') else None
        }
    
    def get_memory_info(self):
        """Get memory usage information"""
        virtual_mem = psutil.virtual_memory()
        swap_mem = psutil.swap_memory()
        
        return {
            'virtual_memory': {
                'total': virtual_mem.total,
                'available': virtual_mem.available,
                'percent': virtual_mem.percent,
                'used': virtual_mem.used,
                'free': virtual_mem.free
            },
            'swap_memory': {
                'total': swap_mem.total,
                'used': swap_mem.used,
                'free': swap_mem.free,
                'percent': swap_mem.percent
            }
        }
    
    def get_disk_info(self):
        """Get disk usage information"""
        disk_info = []
        
        for partition in psutil.disk_partitions():
            try:
                partition_usage = psutil.disk_usage(partition.mountpoint)
                disk_info.append({
                    'device': partition.device,
                    'mountpoint': partition.mountpoint,
                    'file_system': partition.fstype,
                    'total': partition_usage.total,
                    'used': partition_usage.used,
                    'free': partition_usage.free,
                    'percent': partition_usage.used / partition_usage.total * 100
                })
            except PermissionError:
                continue
        
        return disk_info
    
    def get_network_info(self):
        """Get network interface information"""
        network_info = []
        
        for interface, addresses in psutil.net_if_addrs().items():
            interface_info = {
                'interface': interface,
                'addresses': []
            }
            
            for addr in addresses:
                interface_info['addresses'].append({
                    'family': str(addr.family),
                    'address': addr.address,
                    'netmask': addr.netmask,
                    'broadcast': addr.broadcast
                })
            
            network_info.append(interface_info)
        
        return network_info
    
    def get_running_processes(self, limit=10):
        """Get information about running processes"""
        processes = []
        
        for proc in psutil.process_iter(['pid', 'name', 'cpu_percent', 'memory_percent', 'status']):
            try:
                processes.append(proc.info)
            except (psutil.NoSuchProcess, psutil.AccessDenied):
                pass
        
        # Sort by CPU usage
        processes.sort(key=lambda x: x['cpu_percent'] or 0, reverse=True)
        return processes[:limit]
    
    def check_system_health(self):
        """Perform basic system health checks"""
        health_status = {
            'status': 'healthy',
            'issues': [],
            'warnings': []
        }
        
        # Check CPU usage
        cpu_percent = psutil.cpu_percent(interval=1)
        if cpu_percent > 90:
            health_status['issues'].append(f"High CPU usage: {cpu_percent}%")
            health_status['status'] = 'warning'
        elif cpu_percent > 80:
            health_status['warnings'].append(f"Elevated CPU usage: {cpu_percent}%")
        
        # Check memory usage
        memory = psutil.virtual_memory()
        if memory.percent > 90:
            health_status['issues'].append(f"High memory usage: {memory.percent}%")
            health_status['status'] = 'critical'
        elif memory.percent > 80:
            health_status['warnings'].append(f"Elevated memory usage: {memory.percent}%")
        
        # Check disk usage
        for partition in psutil.disk_partitions():
            try:
                usage = psutil.disk_usage(partition.mountpoint)
                percent_used = usage.used / usage.total * 100
                
                if percent_used > 95:
                    health_status['issues'].append(f"Disk {partition.mountpoint} almost full: {percent_used:.1f}%")
                    health_status['status'] = 'critical'
                elif percent_used > 85:
                    health_status['warnings'].append(f"Disk {partition.mountpoint} getting full: {percent_used:.1f}%")
            except PermissionError:
                continue
        
        return health_status
    
    def generate_report(self, output_file=None):
        """Generate comprehensive system report"""
        report = {
            'report_timestamp': datetime.now().isoformat(),
            'system_info': self.get_system_info(),
            'cpu_info': self.get_cpu_info(),
            'memory_info': self.get_memory_info(),
            'disk_info': self.get_disk_info(),
            'network_info': self.get_network_info(),
            'top_processes': self.get_running_processes(),
            'health_check': self.check_system_health()
        }
        
        if output_file:
            with open(output_file, 'w') as f:
                json.dump(report, f, indent=2, default=str)
            print(f"Report saved to {output_file}")
        
        return report

# Usage example
def monitor_system():
    """Monitor system and generate alerts if needed"""
    monitor = SystemMonitor()
    
    # Check system health
    health = monitor.check_system_health()
    print(f"System Status: {health['status']}")
    
    if health['issues']:
        print("Critical Issues:")
        for issue in health['issues']:
            print(f"  ⚠️  {issue}")
    
    if health['warnings']:
        print("Warnings:")
        for warning in health['warnings']:
            print(f"  ⚡ {warning}")
    
    # Generate detailed report
    report_file = f"system_report_{datetime.now().strftime('%Y%m%d_%H%M%S')}.json"
    monitor.generate_report(report_file)

# Run monitoring
# monitor_system()
```

### Process Automation and Task Scheduling

**Real-World Application**: Automating recurring tasks

```python
import schedule
import time
import subprocess
import logging
from datetime import datetime, timedelta
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
import os

# Set up logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('automation.log'),
        logging.StreamHandler()
    ]
)

class TaskAutomator:
    def __init__(self):
        self.logger = logging.getLogger(__name__)
        self.tasks_run = []
    
    def backup_directory(self, source_dir, backup_dir, compress=True):
        """Create backup of directory"""
        try:
            timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
            backup_name = f"backup_{timestamp}"
            
            if compress:
                backup_path = f"{backup_dir}/{backup_name}.tar.gz"
                cmd = f"tar -czf {backup_path} -C {os.path.dirname(source_dir)} {os.path.basename(source_dir)}"
            else:
                backup_path = f"{backup_dir}/{backup_name}"
                cmd = f"cp -r {source_dir} {backup_path}"
            
            result = subprocess.run(cmd, shell=True, capture_output=True, text=True)
            
            if result.returncode == 0:
                self.logger.info(f"Backup created successfully: {backup_path}")
                return backup_path
            else:
                self.logger.error(f"Backup failed: {result.stderr}")
                return None
                
        except Exception as e:
            self.logger.error(f"Backup error: {e}")
            return None
    
    def cleanup_old_files(self, directory, days_old=30, file_pattern="*"):
        """Remove files older than specified days"""
        try:
            cutoff_date = datetime.now() - timedelta(days=days_old)
            removed_files = []
            
            for file_path in Path(directory).glob(file_pattern):
                if file_path.is_file():
                    file_time = datetime.fromtimestamp(file_path.stat().st_mtime)
                    if file_time < cutoff_date:
                        file_path.unlink()
                        removed_files.append(str(file_path))
            
            self.logger.info(f"Cleaned up {len(removed_files)} old files from {directory}")
            return removed_files
            
        except Exception as e:
            self.logger.error(f"Cleanup error: {e}")
            return []
    
    def send_notification_email(self, subject, body, to_email, smtp_server="smtp.gmail.com", smtp_port=587):
        """Send notification email"""
        try:
            # Email configuration (use environment variables in production)
            from_email = os.getenv('EMAIL_USER', 'your_email@gmail.com')
            password = os.getenv('EMAIL_PASSWORD', 'your_app_password')
            
            msg = MIMEMultipart()
            msg['From'] = from_email
            msg['To'] = to_email
            msg['Subject'] = subject
            
            msg.attach(MIMEText(body, 'plain'))
            
            server = smtplib.SMTP(smtp_server, smtp_port)
            server.starttls()
            server.login(from_email, password)
            text = msg.as_string()
            server.sendmail(from_email, to_email, text)
            server.quit()
            
            self.logger.info(f"Notification email sent to {to_email}")
            return True
            
        except Exception as e:
            self.logger.error(f"Email error: {e}")
            return False
    
    def check_disk_space(self, path="/", threshold=85):
        """Check disk space and alert if threshold exceeded"""
        try:
            usage = psutil.disk_usage(path)
            percent_used = (usage.used / usage.total) * 100
            
            if percent_used > threshold:
                message = f"Disk space warning: {path} is {percent_used:.1f}% full"
                self.logger.warning(message)
                
                # Send email notification
                self.send_notification_email(
                    subject="Disk Space Alert",
                    body=message,
                    to_email="admin@example.com"
                )
                
                return False
            else:
                self.logger.info(f"Disk space OK: {path} is {percent_used:.1f}% full")
                return True
                
        except Exception as e:
            self.logger.error(f"Disk check error: {e}")
            return False
    
    def update_system_packages(self):
        """Update system packages (Linux/macOS)"""
        try:
            system = platform.system().lower()
            
            if system == "linux":
                # Ubuntu/Debian
                if shutil.which("apt"):
                    cmd = "sudo apt update && sudo apt upgrade -y"
                # CentOS/RHEL
                elif shutil.which("yum"):
                    cmd = "sudo yum update -y"
                else:
                    self.logger.warning("No supported package manager found")
                    return False
            elif system == "darwin":  # macOS
                if shutil.which("brew"):
                    cmd = "brew update && brew upgrade"
                else:
                    self.logger.warning("Homebrew not found")
                    return False
            else:
                self.logger.warning(f"Unsupported system: {system}")
                return False
            
            result = subprocess.run(cmd, shell=True, capture_output=True, text=True)
            
            if result.returncode == 0:
                self.logger.info("System packages updated successfully")
                return True
            else:
                self.logger.error(f"Package update failed: {result.stderr}")
                return False
                
        except Exception as e:
            self.logger.error(f"Package update error: {e}")
            return False
    
    def run_scheduled_tasks(self):
        """Run all scheduled maintenance tasks"""
        self.logger.info("Starting scheduled maintenance tasks")
        
        tasks = [
            ("Disk Space Check", lambda: self.check_disk_space()),
            ("Backup Documents", lambda: self.backup_directory(
                "/home/user/Documents", 
                "/home/user/Backups"
            )),
            ("Cleanup Temp Files", lambda: self.cleanup_old_files(
                "/tmp", 
                days_old=7
            )),
            ("System Update", lambda: self.update_system_packages())
        ]
        
        results = []
        for task_name, task_func in tasks:
            try:
                self.logger.info(f"Running task: {task_name}")
                result = task_func()
                results.append((task_name, "Success" if result else "Failed"))
                self.tasks_run.append({
                    'task': task_name,
                    'timestamp': datetime.now().isoformat(),
                    'result': result
                })
            except Exception as e:
                self.logger.error(f"Task {task_name} failed: {e}")
                results.append((task_name, f"Error: {e}"))
        
        # Send summary email
        summary = "\n".join([f"{task}: {result}" for task, result in results])
        self.send_notification_email(
            subject="Scheduled Maintenance Summary",
            body=f"Maintenance tasks completed:\n\n{summary}",
            to_email="admin@example.com"
        )
        
        self.logger.info("Scheduled maintenance tasks completed")

# Task scheduler setup
def setup_scheduler():
    """Set up automated task scheduling"""
    automator = TaskAutomator()
    
    # Schedule daily tasks
    schedule.every().day.at("02:00").do(automator.run_scheduled_tasks)
    
    # Schedule weekly tasks
    schedule.every().sunday.at("03:00").do(
        automator.backup_directory,
        "/home/user/Projects",
        "/home/user/Backups/weekly"
    )
    
    # Schedule hourly checks
    schedule.every().hour.do(automator.check_disk_space)
    
    print("Scheduler set up. Running tasks...")
    
    while True:
        schedule.run_pending()
        time.sleep(60)  # Check every minute

# Command-line interface
def main():
    """Main function for command-line usage"""
    import argparse
    
    parser = argparse.ArgumentParser(description="System Automation Tool")
    parser.add_argument("--backup", nargs=2, metavar=("SOURCE", "DEST"),
                       help="Backup directory")
    parser.add_argument("--cleanup", nargs=2, metavar=("DIR", "DAYS"),
                       help="Cleanup old files")
    parser.add_argument("--disk-check", nargs="?", const="/", metavar="PATH",
                       help="Check disk space")
    parser.add_argument("--schedule", action="store_true",
                       help="Run scheduler")
    parser.add_argument("--maintenance", action="store_true",
                       help="Run maintenance tasks")
    
    args = parser.parse_args()
    automator = TaskAutomator()
    
    if args.backup:
        source, dest = args.backup
        automator.backup_directory(source, dest)
    
    elif args.cleanup:
        directory, days = args.cleanup
        automator.cleanup_old_files(directory, int(days))
    
    elif args.disk_check is not None:
        automator.check_disk_space(args.disk_check)
    
    elif args.schedule:
        setup_scheduler()
    
    elif args.maintenance:
        automator.run_scheduled_tasks()
    
    else:
        parser.print_help()

if __name__ == "__main__":
    main()
```

## Phase 2: Web Scraping and Data Extraction

### Web Scraping with Requests and BeautifulSoup

**Real-World Application**: Extracting data from websites

```python
import requests
from bs4 import BeautifulSoup
import csv
import json
import time
import random
from urllib.parse import urljoin, urlparse
import logging

class WebScraper:
    def __init__(self, delay_range=(1, 3), user_agent=None):
        self.session = requests.Session()
        self.delay_range = delay_range
        self.logger = logging.getLogger(__name__)
        
        # Set user agent
        if user_agent:
            self.session.headers.update({'User-Agent': user_agent})
        else:
            self.session.headers.update({
                'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
            })
    
    def get_page(self, url, retries=3):
        """Get page content with error handling and retries"""
        for attempt in range(retries):
            try:
                response = self.session.get(url, timeout=10)
                response.raise_for_status()
                
                # Random delay to be respectful
                delay = random.uniform(*self.delay_range)
                time.sleep(delay)
                
                return response
                
            except requests.RequestException as e:
                self.logger.warning(f"Attempt {attempt + 1} failed for {url}: {e}")
                if attempt == retries - 1:
                    self.logger.error(f"Failed to fetch {url} after {retries} attempts")
                    return None
                time.sleep(2 ** attempt)  # Exponential backoff
        
        return None
    
    def scrape_news_articles(self, base_url, max_articles=50):
        """Scrape news articles from a news website"""
        articles = []
        
        response = self.get_page(base_url)
        if not response:
            return articles
        
        soup = BeautifulSoup(response.content, 'html.parser')
        
        # Find article links (this would need to be customized per site)
        article_links = soup.find_all('a', href=True)
        article_urls = []
        
        for link in article_links:
            href = link.get('href')
            if href and '/article/' in href:  # Example pattern
                full_url = urljoin(base_url, href)
                article_urls.append(full_url)
        
        # Remove duplicates and limit
        article_urls = list(set(article_urls))[:max_articles]
        
        for url in article_urls:
            article_data = self.scrape_article(url)
            if article_data:
                articles.append(article_data)
        
        return articles
    
    def scrape_article(self, url):
        """Scrape individual article content"""
        response = self.get_page(url)
        if not response:
            return None
        
        soup = BeautifulSoup(response.content, 'html.parser')
        
        try:
            # Extract article data (customize selectors for each site)
            title = soup.find('h1').get_text(strip=True) if soup.find('h1') else 'No title'
            
            # Try multiple selectors for content
            content_selectors = ['.article-content', '.post-content', 'article', '.content']
            content = 'No content found'
            
            for selector in content_selectors:
                content_elem = soup.select_one(selector)
                if content_elem:
                    content = content_elem.get_text(strip=True)
                    break
            
            # Extract metadata
            meta_description = soup.find('meta', attrs={'name': 'description'})
            description = meta_description.get('content') if meta_description else ''
            
            # Extract publish date
            date_selectors = ['time', '.publish-date', '.date']
            publish_date = ''
            
            for selector in date_selectors:
                date_elem = soup.select_one(selector)
                if date_elem:
                    publish_date = date_elem.get_text(strip=True)
                    break
            
            return {
                'url': url,
                'title': title,
                'content': content[:500] + '...' if len(content) > 500 else content,
                'description': description,
                'publish_date': publish_date,
                'scraped_at': time.strftime('%Y-%m-%d %H:%M:%S')
            }
            
        except Exception as e:
            self.logger.error(f"Error scraping article {url}: {e}")
            return None
    
    def scrape_product_listings(self, search_url, max_products=100):
        """Scrape product listings from e-commerce site"""
        products = []
        page = 1
        
        while len(products) < max_products:
            # Construct page URL
            page_url = f"{search_url}&page={page}"
            response = self.get_page(page_url)
            
            if not response:
                break
            
            soup = BeautifulSoup(response.content, 'html.parser')
            
            # Find product containers (customize for each site)
            product_containers = soup.find_all('div', class_='product-item')
            
            if not product_containers:
                break  # No more products
            
            for container in product_containers:
                try:
                    # Extract product data
                    name_elem = container.find('h3') or container.find('.product-name')
                    name = name_elem.get_text(strip=True) if name_elem else 'Unknown'
                    
                    price_elem = container.find('.price') or container.find('.cost')
                    price = price_elem.get_text(strip=True) if price_elem else 'N/A'
                    
                    link_elem = container.find('a', href=True)
                    link = urljoin(search_url, link_elem['href']) if link_elem else ''
                    
                    rating_elem = container.find('.rating') or container.find('.stars')
                    rating = rating_elem.get_text(strip=True) if rating_elem else 'N/A'
                    
                    products.append({
                        'name': name,
                        'price': price,
                        'rating': rating,
                        'link': link,
                        'scraped_at': time.strftime('%Y-%m-%d %H:%M:%S')
                    })
                    
                    if len(products) >= max_products:
                        break
                        
                except Exception as e:
                    self.logger.error(f"Error extracting product data: {e}")
                    continue
            
            page += 1
            
            # Safety check to avoid infinite loops
            if page > 50:
                break
        
        return products
    
    def save_to_csv(self, data, filename):
        """Save scraped data to CSV file"""
        if not data:
            self.logger.warning("No data to save")
            return
        
        with open(filename, 'w', newline='', encoding='utf-8') as csvfile:
            fieldnames = data[0].keys()
            writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
            
            writer.writeheader()
            for row in data:
                writer.writerow(row)
        
        self.logger.info(f"Data saved to {filename}")
    
    def save_to_json(self, data, filename):
        """Save scraped data to JSON file"""
        with open(filename, 'w', encoding='utf-8') as jsonfile:
            json.dump(data, jsonfile, indent=2, ensure_ascii=False)
        
        self.logger.info(f"Data saved to {filename}")

# Usage examples
def scrape_news_example():
    """Example: Scrape news articles"""
    scraper = WebScraper(delay_range=(2, 4))
    
    # Example news site (replace with actual site)
    articles = scraper.scrape_news_articles('https://example-news.com', max_articles=20)
    
    if articles:
        scraper.save_to_csv(articles, 'news_articles.csv')
        scraper.save_to_json(articles, 'news_articles.json')
        print(f"Scraped {len(articles)} articles")
    else:
        print("No articles found")

def scrape_products_example():
    """Example: Scrape product listings"""
    scraper = WebScraper()
    
    # Example product search (replace with actual site)
    products = scraper.scrape_product_listings(
        'https://example-shop.com/search?q=laptop',
        max_products=50
    )
    
    if products:
        scraper.save_to_csv(products, 'products.csv')
        print(f"Scraped {len(products)} products")
    else:
        print("No products found")

# Advanced scraping with Selenium (for JavaScript-heavy sites)
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.options import Options

class AdvancedScraper:
    def __init__(self, headless=True):
        self.options = Options()
        if headless:
            self.options.add_argument('--headless')
        self.options.add_argument('--no-sandbox')
        self.options.add_argument('--disable-dev-shm-usage')
        self.driver = None
    
    def start_driver(self):
        """Start the Chrome driver"""
        self.driver = webdriver.Chrome(options=self.options)
        return self.driver
    
    def close_driver(self):
        """Close the driver"""
        if self.driver:
            self.driver.quit()
    
    def scrape_dynamic_content(self, url, wait_selector):
        """Scrape content that loads dynamically with JavaScript"""
        if not self.driver:
            self.start_driver()
        
        try:
            self.driver.get(url)
            
            # Wait for content to load
            wait = WebDriverWait(self.driver, 10)
            wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, wait_selector)))
            
            # Get page source after JavaScript execution
            soup = BeautifulSoup(self.driver.page_source, 'html.parser')
            
            return soup
            
        except Exception as e:
            logging.error(f"Error scraping dynamic content: {e}")
            return None
    
    def scrape_infinite_scroll(self, url, scroll_selector, max_scrolls=10):
        """Scrape content from infinite scroll pages"""
        if not self.driver:
            self.start_driver()
        
        try:
            self.driver.get(url)
            
            # Scroll down multiple times to load more content
            for i in range(max_scrolls):
                # Scroll to bottom
                self.driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
                
                # Wait for new content to load
                time.sleep(2)
                
                # Check if we've reached the end
                try:
                    WebDriverWait(self.driver, 5).until(
                        EC.presence_of_element_located((By.CSS_SELECTOR, scroll_selector))
                    )
                except:
                    break  # No more content to load
            
            # Get final page source
            soup = BeautifulSoup(self.driver.page_source, 'html.parser')
            return soup
            
        except Exception as e:
            logging.error(f"Error scraping infinite scroll: {e}")
            return None

# Example usage
def main():
    """Main function demonstrating scraping capabilities"""
    # Basic scraping example
    print("Starting web scraping examples...")
    
    # Uncomment to run examples
    # scrape_news_example()
    # scrape_products_example()
    
    print("Web scraping examples completed")

if __name__ == "__main__":
    main()
```

This covers the first major sections of the Automation & Scripting specialization. The content includes practical, real-world examples that developers can immediately use and adapt for their own automation needs. Each section builds upon the previous one and focuses on solving actual problems rather than just demonstrating syntax.

<function_calls>
<invoke name="taskStatus">
<parameter name="status">completed