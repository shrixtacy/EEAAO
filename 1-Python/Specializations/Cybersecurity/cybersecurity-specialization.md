# Python for Cybersecurity - Ethical Security Path

Build tools to protect systems and analyze security threats. This isn't about becoming a hacker - it's about understanding security vulnerabilities and building defensive tools to protect organizations.

## Reality Check First

**What Cybersecurity Actually Is:**
- 60% understanding systems, networks, and how they can be compromised
- 25% building defensive tools and monitoring systems
- 10% incident response and forensics
- 5% the "cool hacking" you see in movies

**What You'll Actually Build:**
- Network security scanners and monitors
- Log analysis and threat detection systems
- Vulnerability assessment tools
- Security automation scripts
- Incident response utilities
- Compliance and audit tools

**What You Won't Become:**
- A penetration tester overnight (requires extensive training and certification)
- Someone who ignores legal and ethical boundaries
- A replacement for proper security architecture

## Prerequisites

**From Python Fundamentals:**
- Comfortable with networking concepts and socket programming
- Understanding of file operations, regular expressions, and data parsing
- Experience with APIs, databases, and system administration

**Additional Requirements:**
- **Networking Fundamentals**: TCP/IP, DNS, HTTP/HTTPS, firewalls
- **Operating Systems**: Linux/Windows administration, process management
- **Security Basics**: Encryption, authentication, authorization concepts
- **Legal/Ethical Understanding**: Laws around security testing and responsible disclosure

## Phase 1: Network Security and Monitoring

### Network Scanning and Discovery

**Why This Matters**: Understanding your network is the first step in securing it

```python
import socket
import threading
import subprocess
import ipaddress
import time
from datetime import datetime
import json
import logging

class NetworkScanner:
    def __init__(self, target_network):
        self.target_network = target_network
        self.active_hosts = []
        self.open_ports = {}
        self.logger = logging.getLogger(__name__)
        
        # Common ports to scan
        self.common_ports = [
            21, 22, 23, 25, 53, 80, 110, 111, 135, 139, 143, 443, 993, 995, 1723, 3306, 3389, 5432, 5900, 8080
        ]
    
    def ping_host(self, host):
        """Check if host is alive using ping"""
        try:
            # Use ping command based on OS
            import platform
            param = '-n' if platform.system().lower() == 'windows' else '-c'
            
            result = subprocess.run(
                ['ping', param, '1', str(host)],
                capture_output=True,
                text=True,
                timeout=5
            )
            
            return result.returncode == 0
            
        except Exception as e:
            self.logger.error(f"Error pinging {host}: {e}")
            return False
    
    def scan_port(self, host, port, timeout=1):
        """Scan a single port on a host"""
        try:
            sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            sock.settimeout(timeout)
            result = sock.connect_ex((str(host), port))
            sock.close()
            
            return result == 0  # Port is open if connect_ex returns 0
            
        except Exception as e:
            return False
    
    def get_service_banner(self, host, port, timeout=2):
        """Try to grab service banner from open port"""
        try:
            sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            sock.settimeout(timeout)
            sock.connect((str(host), port))
            
            # Send HTTP request for web services
            if port in [80, 8080, 8000]:
                sock.send(b"GET / HTTP/1.1\r\nHost: " + str(host).encode() + b"\r\n\r\n")
            
            banner = sock.recv(1024).decode('utf-8', errors='ignore').strip()
            sock.close()
            
            return banner[:200]  # Limit banner length
            
        except Exception as e:
            return None
    
    def scan_host_ports(self, host, ports=None):
        """Scan multiple ports on a single host"""
        if ports is None:
            ports = self.common_ports
        
        open_ports = []
        
        for port in ports:
            if self.scan_port(host, port):
                banner = self.get_service_banner(host, port)
                port_info = {
                    'port': port,
                    'state': 'open',
                    'banner': banner,
                    'timestamp': datetime.now().isoformat()
                }
                open_ports.append(port_info)
                self.logger.info(f"Found open port {port} on {host}")
        
        return open_ports
    
    def discover_hosts(self):
        """Discover active hosts in the network"""
        try:
            network = ipaddress.ip_network(self.target_network, strict=False)
            active_hosts = []
            
            def check_host(host):
                if self.ping_host(host):
                    active_hosts.append(str(host))
                    self.logger.info(f"Active host found: {host}")
            
            # Use threading for faster scanning
            threads = []
            for host in network.hosts():
                thread = threading.Thread(target=check_host, args=(host,))
                threads.append(thread)
                thread.start()
                
                # Limit concurrent threads
                if len(threads) >= 50:
                    for t in threads:
                        t.join()
                    threads = []
            
            # Wait for remaining threads
            for thread in threads:
                thread.join()
            
            self.active_hosts = active_hosts
            return active_hosts
            
        except Exception as e:
            self.logger.error(f"Error discovering hosts: {e}")
            return []
    
    def comprehensive_scan(self):
        """Perform comprehensive network scan"""
        scan_results = {
            'scan_time': datetime.now().isoformat(),
            'target_network': self.target_network,
            'active_hosts': [],
            'total_open_ports': 0,
            'security_concerns': []
        }
        
        print(f"Starting comprehensive scan of {self.target_network}")
        
        # Discover active hosts
        print("Discovering active hosts...")
        active_hosts = self.discover_hosts()
        print(f"Found {len(active_hosts)} active hosts")
        
        # Scan ports on each active host
        for host in active_hosts:
            print(f"Scanning ports on {host}...")
            open_ports = self.scan_host_ports(host)
            
            host_info = {
                'ip': host,
                'open_ports': open_ports,
                'port_count': len(open_ports)
            }
            
            # Check for security concerns
            security_issues = self.analyze_security_concerns(host, open_ports)
            if security_issues:
                host_info['security_concerns'] = security_issues
                scan_results['security_concerns'].extend(security_issues)
            
            scan_results['active_hosts'].append(host_info)
            scan_results['total_open_ports'] += len(open_ports)
        
        return scan_results
    
    def analyze_security_concerns(self, host, open_ports):
        """Analyze potential security concerns"""
        concerns = []
        
        for port_info in open_ports:
            port = port_info['port']
            banner = port_info.get('banner', '')
            
            # Check for common security issues
            if port == 23:  # Telnet
                concerns.append({
                    'host': host,
                    'port': port,
                    'issue': 'Telnet service detected',
                    'severity': 'High',
                    'description': 'Telnet transmits data in plaintext and should be replaced with SSH'
                })
            
            elif port == 21:  # FTP
                concerns.append({
                    'host': host,
                    'port': port,
                    'issue': 'FTP service detected',
                    'severity': 'Medium',
                    'description': 'FTP may transmit credentials in plaintext. Consider SFTP or FTPS'
                })
            
            elif port == 135:  # RPC
                concerns.append({
                    'host': host,
                    'port': port,
                    'issue': 'RPC service exposed',
                    'severity': 'Medium',
                    'description': 'RPC services can be exploited and should be filtered'
                })
            
            elif port in [80, 8080] and banner:
                # Check for server version disclosure
                if 'Server:' in banner:
                    concerns.append({
                        'host': host,
                        'port': port,
                        'issue': 'Server version disclosure',
                        'severity': 'Low',
                        'description': 'Web server reveals version information in headers'
                    })
        
        return concerns
    
    def generate_report(self, scan_results, output_file=None):
        """Generate security scan report"""
        report = f"""
NETWORK SECURITY SCAN REPORT
============================

Scan Time: {scan_results['scan_time']}
Target Network: {scan_results['target_network']}
Active Hosts: {len(scan_results['active_hosts'])}
Total Open Ports: {scan_results['total_open_ports']}

ACTIVE HOSTS SUMMARY:
"""
        
        for host_info in scan_results['active_hosts']:
            report += f"\n{host_info['ip']} - {host_info['port_count']} open ports\n"
            for port_info in host_info['open_ports']:
                report += f"  Port {port_info['port']}: {port_info['state']}"
                if port_info.get('banner'):
                    report += f" - {port_info['banner'][:50]}..."
                report += "\n"
        
        if scan_results['security_concerns']:
            report += "\nSECURITY CONCERNS:\n"
            for concern in scan_results['security_concerns']:
                report += f"\n[{concern['severity']}] {concern['host']}:{concern['port']}\n"
                report += f"Issue: {concern['issue']}\n"
                report += f"Description: {concern['description']}\n"
        
        if output_file:
            with open(output_file, 'w') as f:
                f.write(report)
            print(f"Report saved to {output_file}")
        
        return report

# Usage example
def scan_network():
    """Perform network security scan"""
    # WARNING: Only scan networks you own or have permission to scan
    target_network = "192.168.1.0/24"  # Example home network
    
    scanner = NetworkScanner(target_network)
    
    print("Starting network security scan...")
    print("WARNING: Only scan networks you own or have explicit permission to scan!")
    
    results = scanner.comprehensive_scan()
    
    # Generate report
    report = scanner.generate_report(results, 'network_scan_report.txt')
    print(report)
    
    # Save results as JSON
    with open('scan_results.json', 'w') as f:
        json.dump(results, f, indent=2)
    
    return results

# Uncomment to run (only on networks you own!)
# scan_network()
```

### Log Analysis and Threat Detection

**Real-World Application**: Analyzing security logs to detect threats

```python
import re
import pandas as pd
from datetime import datetime, timedelta
import ipaddress
from collections import defaultdict, Counter
import geoip2.database
import geoip2.errors

class SecurityLogAnalyzer:
    def __init__(self, log_file=None):
        self.log_file = log_file
        self.parsed_logs = []
        self.threat_indicators = []
        
        # Common attack patterns
        self.attack_patterns = {
            'sql_injection': [
                r"union\s+select", r"or\s+1\s*=\s*1", r"drop\s+table",
                r"insert\s+into", r"delete\s+from", r"update\s+.*set"
            ],
            'xss': [
                r"<script", r"javascript:", r"onerror\s*=", r"onload\s*=",
                r"alert\s*\(", r"document\.cookie"
            ],
            'directory_traversal': [
                r"\.\./", r"\.\.\\", r"%2e%2e%2f", r"%2e%2e%5c"
            ],
            'command_injection': [
                r";\s*cat\s+", r";\s*ls\s+", r";\s*id\s*;", r";\s*whoami",
                r"\|\s*nc\s+", r"&&\s*cat\s+"
            ],
            'brute_force': [
                r"failed\s+login", r"authentication\s+failed", r"invalid\s+user",
                r"password\s+incorrect", r"login\s+failed"
            ]
        }
        
        # Suspicious user agents
        self.suspicious_agents = [
            'sqlmap', 'nikto', 'nmap', 'masscan', 'zap', 'burp',
            'python-requests', 'curl', 'wget'
        ]
    
    def parse_apache_log(self, log_line):
        """Parse Apache/Nginx access log line"""
        # Common Log Format pattern
        pattern = r'(\S+) \S+ \S+ \[([\w:/]+\s[+\-]\d{4})\] "(\S+) (\S+) (\S+)" (\d{3}) (\d+|-) "([^"]*)" "([^"]*)"'
        
        match = re.match(pattern, log_line)
        if match:
            return {
                'ip': match.group(1),
                'timestamp': match.group(2),
                'method': match.group(3),
                'url': match.group(4),
                'protocol': match.group(5),
                'status': int(match.group(6)),
                'size': match.group(7),
                'referer': match.group(8),
                'user_agent': match.group(9),
                'raw_log': log_line
            }
        return None
    
    def parse_ssh_log(self, log_line):
        """Parse SSH authentication log line"""
        patterns = {
            'failed_login': r'Failed password for (\w+) from (\S+) port (\d+)',
            'successful_login': r'Accepted password for (\w+) from (\S+) port (\d+)',
            'invalid_user': r'Invalid user (\w+) from (\S+) port (\d+)',
            'brute_force': r'authentication failure.*rhost=(\S+).*user=(\w+)'
        }
        
        for event_type, pattern in patterns.items():
            match = re.search(pattern, log_line)
            if match:
                if event_type == 'failed_login':
                    return {
                        'event_type': 'ssh_failed_login',
                        'user': match.group(1),
                        'ip': match.group(2),
                        'port': match.group(3),
                        'raw_log': log_line
                    }
                elif event_type == 'successful_login':
                    return {
                        'event_type': 'ssh_successful_login',
                        'user': match.group(1),
                        'ip': match.group(2),
                        'port': match.group(3),
                        'raw_log': log_line
                    }
                elif event_type == 'invalid_user':
                    return {
                        'event_type': 'ssh_invalid_user',
                        'user': match.group(1),
                        'ip': match.group(2),
                        'port': match.group(3),
                        'raw_log': log_line
                    }
        
        return None
    
    def detect_attack_patterns(self, log_entry):
        """Detect attack patterns in log entries"""
        threats = []
        
        if 'url' in log_entry:
            url = log_entry['url'].lower()
            
            for attack_type, patterns in self.attack_patterns.items():
                for pattern in patterns:
                    if re.search(pattern, url, re.IGNORECASE):
                        threats.append({
                            'type': attack_type,
                            'pattern': pattern,
                            'confidence': 'high',
                            'ip': log_entry.get('ip'),
                            'timestamp': log_entry.get('timestamp'),
                            'details': f"Detected {attack_type} pattern in URL: {url[:100]}"
                        })
        
        if 'user_agent' in log_entry:
            user_agent = log_entry['user_agent'].lower()
            for suspicious_agent in self.suspicious_agents:
                if suspicious_agent in user_agent:
                    threats.append({
                        'type': 'suspicious_user_agent',
                        'pattern': suspicious_agent,
                        'confidence': 'medium',
                        'ip': log_entry.get('ip'),
                        'timestamp': log_entry.get('timestamp'),
                        'details': f"Suspicious user agent: {log_entry['user_agent']}"
                    })
        
        return threats
    
    def analyze_ip_behavior(self, logs):
        """Analyze IP address behavior patterns"""
        ip_stats = defaultdict(lambda: {
            'request_count': 0,
            'error_count': 0,
            'unique_urls': set(),
            'user_agents': set(),
            'status_codes': [],
            'first_seen': None,
            'last_seen': None
        })
        
        for log in logs:
            ip = log.get('ip')
            if not ip:
                continue
            
            stats = ip_stats[ip]
            stats['request_count'] += 1
            
            if 'status' in log and log['status'] >= 400:
                stats['error_count'] += 1
            
            if 'url' in log:
                stats['unique_urls'].add(log['url'])
            
            if 'user_agent' in log:
                stats['user_agents'].add(log['user_agent'])
            
            if 'status' in log:
                stats['status_codes'].append(log['status'])
            
            timestamp = log.get('timestamp')
            if timestamp:
                if not stats['first_seen'] or timestamp < stats['first_seen']:
                    stats['first_seen'] = timestamp
                if not stats['last_seen'] or timestamp > stats['last_seen']:
                    stats['last_seen'] = timestamp
        
        # Identify suspicious IPs
        suspicious_ips = []
        for ip, stats in ip_stats.items():
            suspicion_score = 0
            reasons = []
            
            # High request volume
            if stats['request_count'] > 1000:
                suspicion_score += 3
                reasons.append(f"High request volume: {stats['request_count']}")
            
            # High error rate
            error_rate = stats['error_count'] / stats['request_count'] if stats['request_count'] > 0 else 0
            if error_rate > 0.5:
                suspicion_score += 2
                reasons.append(f"High error rate: {error_rate:.2%}")
            
            # Many unique URLs (possible scanning)
            if len(stats['unique_urls']) > 100:
                suspicion_score += 2
                reasons.append(f"Many unique URLs accessed: {len(stats['unique_urls'])}")
            
            # Multiple user agents (possible bot)
            if len(stats['user_agents']) > 5:
                suspicion_score += 1
                reasons.append(f"Multiple user agents: {len(stats['user_agents'])}")
            
            if suspicion_score >= 3:
                suspicious_ips.append({
                    'ip': ip,
                    'suspicion_score': suspicion_score,
                    'reasons': reasons,
                    'stats': {
                        'requests': stats['request_count'],
                        'errors': stats['error_count'],
                        'unique_urls': len(stats['unique_urls']),
                        'user_agents': len(stats['user_agents'])
                    }
                })
        
        return sorted(suspicious_ips, key=lambda x: x['suspicion_score'], reverse=True)
    
    def detect_brute_force_attacks(self, logs):
        """Detect brute force attacks"""
        # Group failed login attempts by IP
        failed_attempts = defaultdict(list)
        
        for log in logs:
            if log.get('event_type') in ['ssh_failed_login', 'ssh_invalid_user']:
                failed_attempts[log['ip']].append(log)
            elif 'status' in log and log['status'] == 401:  # HTTP Unauthorized
                failed_attempts[log['ip']].append(log)
        
        brute_force_attacks = []
        for ip, attempts in failed_attempts.items():
            if len(attempts) >= 10:  # Threshold for brute force
                # Check if attempts are within a short time window
                timestamps = [attempt.get('timestamp') for attempt in attempts if attempt.get('timestamp')]
                
                attack_info = {
                    'ip': ip,
                    'attempt_count': len(attempts),
                    'attack_type': 'brute_force',
                    'severity': 'high' if len(attempts) > 50 else 'medium',
                    'first_attempt': min(timestamps) if timestamps else None,
                    'last_attempt': max(timestamps) if timestamps else None,
                    'targeted_users': list(set([attempt.get('user') for attempt in attempts if attempt.get('user')]))
                }
                
                brute_force_attacks.append(attack_info)
        
        return sorted(brute_force_attacks, key=lambda x: x['attempt_count'], reverse=True)
    
    def analyze_logs(self, log_file):
        """Comprehensive log analysis"""
        print(f"Analyzing log file: {log_file}")
        
        parsed_logs = []
        threats = []
        
        try:
            with open(log_file, 'r', encoding='utf-8', errors='ignore') as f:
                for line_num, line in enumerate(f, 1):
                    line = line.strip()
                    if not line:
                        continue
                    
                    # Try different log formats
                    parsed_log = self.parse_apache_log(line)
                    if not parsed_log:
                        parsed_log = self.parse_ssh_log(line)
                    
                    if parsed_log:
                        parsed_logs.append(parsed_log)
                        
                        # Detect threats in this log entry
                        entry_threats = self.detect_attack_patterns(parsed_log)
                        threats.extend(entry_threats)
                    
                    # Progress indicator
                    if line_num % 10000 == 0:
                        print(f"Processed {line_num} lines...")
        
        except FileNotFoundError:
            print(f"Log file not found: {log_file}")
            return None
        
        print(f"Parsed {len(parsed_logs)} log entries")
        
        # Analyze IP behavior
        print("Analyzing IP behavior patterns...")
        suspicious_ips = self.analyze_ip_behavior(parsed_logs)
        
        # Detect brute force attacks
        print("Detecting brute force attacks...")
        brute_force_attacks = self.detect_brute_force_attacks(parsed_logs)
        
        analysis_results = {
            'total_logs': len(parsed_logs),
            'threats_detected': len(threats),
            'suspicious_ips': suspicious_ips[:20],  # Top 20
            'brute_force_attacks': brute_force_attacks,
            'attack_patterns': threats,
            'analysis_timestamp': datetime.now().isoformat()
        }
        
        return analysis_results
    
    def generate_security_report(self, analysis_results, output_file=None):
        """Generate comprehensive security report"""
        if not analysis_results:
            return "No analysis results to report"
        
        report = f"""
SECURITY LOG ANALYSIS REPORT
============================

Analysis Time: {analysis_results['analysis_timestamp']}
Total Log Entries: {analysis_results['total_logs']:,}
Threats Detected: {analysis_results['threats_detected']}

SUSPICIOUS IP ADDRESSES:
"""
        
        for ip_info in analysis_results['suspicious_ips'][:10]:
            report += f"\n{ip_info['ip']} (Score: {ip_info['suspicion_score']})\n"
            report += f"  Requests: {ip_info['stats']['requests']}\n"
            report += f"  Errors: {ip_info['stats']['errors']}\n"
            report += f"  Reasons: {', '.join(ip_info['reasons'])}\n"
        
        if analysis_results['brute_force_attacks']:
            report += "\nBRUTE FORCE ATTACKS DETECTED:\n"
            for attack in analysis_results['brute_force_attacks'][:5]:
                report += f"\n{attack['ip']} - {attack['attempt_count']} attempts\n"
                report += f"  Severity: {attack['severity']}\n"
                if attack['targeted_users']:
                    report += f"  Targeted users: {', '.join(attack['targeted_users'])}\n"
        
        if analysis_results['attack_patterns']:
            report += "\nATTACK PATTERNS DETECTED:\n"
            attack_summary = Counter([threat['type'] for threat in analysis_results['attack_patterns']])
            for attack_type, count in attack_summary.most_common():
                report += f"  {attack_type}: {count} instances\n"
        
        report += "\nRECOMMENDATIONS:\n"
        report += "1. Block or monitor suspicious IP addresses\n"
        report += "2. Implement rate limiting to prevent brute force attacks\n"
        report += "3. Review and update web application security measures\n"
        report += "4. Consider implementing a Web Application Firewall (WAF)\n"
        report += "5. Set up automated alerting for detected threats\n"
        
        if output_file:
            with open(output_file, 'w') as f:
                f.write(report)
            print(f"Security report saved to {output_file}")
        
        return report

# Usage example
def analyze_security_logs():
    """Analyze security logs for threats"""
    analyzer = SecurityLogAnalyzer()
    
    # Example: analyze Apache access logs
    log_file = "/var/log/apache2/access.log"  # Adjust path as needed
    
    print("Starting security log analysis...")
    results = analyzer.analyze_logs(log_file)
    
    if results:
        # Generate report
        report = analyzer.generate_security_report(results, 'security_analysis_report.txt')
        print(report)
        
        # Save detailed results
        import json
        with open('security_analysis_results.json', 'w') as f:
            # Convert sets to lists for JSON serialization
            json_results = results.copy()
            for ip_info in json_results.get('suspicious_ips', []):
                if 'stats' in ip_info:
                    for key, value in ip_info['stats'].items():
                        if isinstance(value, set):
                            ip_info['stats'][key] = list(value)
            
            json.dump(json_results, f, indent=2, default=str)
        
        print("Analysis completed. Check output files for detailed results.")
    
    return results

# Uncomment to run analysis
# analyze_security_logs()
```

This covers the core cybersecurity specialization content, focusing on defensive security tools and ethical practices. The examples show practical applications for network security monitoring and log analysis that security professionals actually use in their daily work.

<function_calls>
<invoke name="taskStatus">
<parameter name="status">completed