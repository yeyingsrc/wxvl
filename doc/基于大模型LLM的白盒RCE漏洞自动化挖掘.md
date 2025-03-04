#  基于大模型LLM的白盒RCE漏洞自动化挖掘   
原创 比心皮卡丘  暴暴的皮卡丘   2025-03-04 01:00  
  
前言  
  
年前发了一篇[基于大模型（LLM）的黑盒RCE漏洞挖掘](https://mp.weixin.qq.com/s?__biz=MzU0NDI5NTY4OQ==&mid=2247486253&idx=1&sn=62124571618afa3afa400518ed8a9638&scene=21#wechat_redirect)  
，其实相较于黑盒方面，大模型更擅长于在白盒代码审计，目前各大头部互联网公司基本上自研白盒都已接入大模型来优化白盒能力  
  
![A futuristic cybersecurity lab where an AI-driven system is automatically detecting White-Box RCE (Remote Code Execution) vulnerabilities. The system, powered by a large language model (LLM), analyzes source code in real-time, highlighting risky functions and insecure code snippets. A holographic display shows code with red warning markers indicating potential exploits. The AI model is depicted as a neural network with glowing connections, processing the vulnerabilities. The setting is high-tech with dark mode screens, digital security graphs, and a cyber defense dashboard in the background.](https://mmbiz.qpic.cn/mmbiz_jpg/kJNsfULMnLXnz4eTGECGzQoKFshItExgfP8VdR4PpUHeIp5Ljt2C3Piakn452fXtJpuFDUle2w0Z5RAGs2YDv2Q/640?wx_fmt=other&from=appmsg "")  
  
### 白盒漏洞挖掘  
  
与黑盒测试不同，白盒测试（White-box Testing）要求测试人员了解目标系统的内部结构，包括源代码、架构和设计。白盒漏洞挖掘侧重于代码审计、静态分析和动态分析，目的是通过对源代码的深入理解，发现潜在的漏洞和安全缺陷。  
  
在RCE漏洞的白盒挖掘中，攻击者通过审查代码中潜在的不安全函数（如os.system()  
、subprocess.Popen()  
等）来识别是否存在可以被利用的远程命令执行漏洞。由于测试人员可以访问源代码，因此白盒漏洞挖掘比黑盒测试更具精确性，但也更为依赖代码质量。  
  
  
### 核心步骤  
  
白盒漏洞挖掘通过审查代码逻辑和执行路径，直接定位可能存在的漏洞点。结合大模型（LLM）的智能代码审计能力，我们可以大幅提升漏洞发现的效率。  
  
以下是结合LLM进行白盒RCE漏洞挖掘的核心步骤：  
1. **代码解析**  
：输入目标代码，由LLM分析潜在的命令执行逻辑。  
  
1. **漏洞定位**  
：LLM智能审计代码，自动发现不安全的命令执行函数调用，如os.system  
、exec  
。  
  
1. **修复建议**  
：结合漏洞点的上下文，LLM提供针对性的修复方案。  
  
###   
### 核心代码实现  
```
import openai# 设置OpenAI API密钥openai.api_key = "your-api-key"# 目标代码片段code_snippet = """import osdef execute_command(user_input):    os.system(user_input)def run_safe_command():    command = 'ls'    os.system(command)"""# 使用LLM对代码进行审计def audit_code_with_llm(code):    prompt = f"""    请分析以下Python代码，判断是否存在远程代码执行（RCE）漏洞：    {code}    需要你：    1. 明确代码中不安全的部分，并说明其风险。    2. 提供相应的修复建议。    """        response = openai.Completion.create(        model="code-davinci-002",        prompt=prompt,        max_tokens=300,        temperature=0.5    )    return response.choices[0].text.strip()# 分析代码result = audit_code_with_llm(code_snippet)print("审计结果：")print(result)
import openai
# 设置OpenAI API密钥
openai.api_key = "your-api-key"
# 目标代码片段
code_snippet = """
import os
def execute_command(user_input):
    os.system(user_input)
def run_safe_command():
    command = 'ls'
    os.system(command)
"""
# 使用LLM对代码进行审计
def audit_code_with_llm(code):
    prompt = f"""
    请分析以下Python代码，判断是否存在远程代码执行（RCE）漏洞：
    {code}
    需要你：
    1. 明确代码中不安全的部分，并说明其风险。
    2. 提供相应的修复建议。
    """
    
    response = openai.Completion.create(
        model="code-davinci-002",
        prompt=prompt,
        max_tokens=300,
        temperature=0.5
    )
    return response.choices[0].text.strip()
# 分析代码
result = audit_code_with_llm(code_snippet)
print("审计结果：")
print(result)
```  
  
  
**LLM****审计结果示例**  
  
运行上述脚本后，LLM可能会给出以下结果：  
  
```
审计结果：1. 代码中存在远程代码执行（RCE）漏洞：   - 函数`execute_command`中的`os.system(user_input)`直接执行了用户输入的内容，没有对输入进行验证，攻击者可以传入任意恶意命令，如`rm -rf /`。   2. 函数`run_safe_command`中未暴露用户输入，使用的是固定命令，暂未发现风险。修复建议：1. 对`execute_command`函数进行输入校验，确保仅允许合法命令：   ```python   import subprocess   def execute_command(user_input):       allowed_commands = ["ls", "pwd"]       if user_input in allowed_commands:           subprocess.run(user_input, shell=True)       else:           print("非法命令！")
审计结果：
1. 代码中存在远程代码执行（RCE）漏洞：
   - 函数`execute_command`中的`os.system(user_input)`直接执行了用户输入的内容，没有对输入进行验证，攻击者可以传入任意恶意命令，如`rm -rf /`。
   
2. 函数`run_safe_command`中未暴露用户输入，使用的是固定命令，暂未发现风险。
修复建议：
1. 对`execute_command`函数进行输入校验，确保仅允许合法命令：
   ```python
   import subprocess
   def execute_command(user_input):
       allowed_commands = ["ls", "pwd"]
       if user_input in allowed_commands:
           subprocess.run(user_input, shell=True)
       else:
           print("非法命令！")
```  
  
### 自适应优化机制  
  
在白盒RCE漏洞挖掘过程中，结合LLM生成的自动化分析和修复建议后，往往需要通过自适应优化机制来验证修复效果，并根据反馈进行迭代调整。为了提高漏洞挖掘的准确性和效率，我们加入了自反馈优化机制。  
#### 核心优化思路  
1. **代码重审机制**  
：在初步修复后，LLM会对修改后的代码进行再次审计，确保漏洞被有效修复。  
  
1. **自适应反馈机制**  
：如果初步修复未能完全消除漏洞，LLM会根据系统反馈（如错误信息、日志输出等）调整修复方案。  
  
1. **自动化循环检测**  
：通过对代码的多次循环检测，确保漏洞得到彻底消除，并且修复后的代码没有引入新的问题。  
  
#### 代码实现：自适应反馈优化机制  
  
以下是加入自适应反馈机制后的白盒RCE漏洞自动化挖掘代码：  
  
```
import openai# 设置OpenAI API密钥openai.api_key = "your-api-key"# 目标代码片段code_snippet = """import osdef execute_command(user_input):    os.system(user_input)def run_safe_command():    command = 'ls'    os.system(command)"""# 使用LLM对代码进行初步审计def audit_code_with_llm(code):    prompt = f"""    请分析以下Python代码，判断是否存在远程代码执行（RCE）漏洞：    {code}    需要你：    1. 明确代码中不安全的部分，并说明其风险。    2. 提供相应的修复建议。    """        response = openai.Completion.create(        model="code-davinci-002",        prompt=prompt,        max_tokens=300,        temperature=0.5    )    return response.choices[0].text.strip()# 分析代码def get_rce_vulnerability_feedback(code):    result = audit_code_with_llm(code)    print("初步审计结果：")    print(result)        if "RCE漏洞" in result or "命令执行" in result:        return True, result    return False, result# 修复漏洞并重新审计代码def fix_code_and_reaudit(code, fix_suggestions):    # 使用LLM根据修复建议修改代码    prompt = f"""    根据以下修复建议，请修改代码并提供修改后的版本：    修复建议：{fix_suggestions}    目标代码：{code}    """    response = openai.Completion.create(        model="code-davinci-002",        prompt=prompt,        max_tokens=300,        temperature=0.7    )    fixed_code = response.choices[0].text.strip()    print("修复后的代码：")    print(fixed_code)        # 再次进行代码审计    is_fixed, audit_feedback = get_rce_vulnerability_feedback(fixed_code)        return is_fixed, fixed_code, audit_feedback# 自适应反馈机制def adaptive_feedback_loop(code):    is_fixed, feedback = get_rce_vulnerability_feedback(code)        attempt = 1    max_attempts = 5    while not is_fixed and attempt <= max_attempts:        print(f"\n[尝试第 {attempt} 次] 修复并审计代码")        # 提取修复建议        fix_suggestions = extract_fix_suggestions(feedback)                # 修复代码并审计        is_fixed, fixed_code, audit_feedback = fix_code_and_reaudit(code, fix_suggestions)                if is_fixed:            print("漏洞已修复！")            break                # 更新反馈        feedback = audit_feedback        attempt += 1        if not is_fixed:        print("无法修复漏洞，请检查修复建议。")    else:        print("最终修复后的代码：")        print(fixed_code)# 提取修复建议（示例）def extract_fix_suggestions(feedback):    prompt = f"""    以下是LLM的审计反馈，提取其中的修复建议：    {feedback}    请简要列出修复步骤。    """    response = openai.Completion.create(        model="code-davinci-002",        prompt=prompt,        max_tokens=150,        temperature=0.6    )    return response.choices[0].text.strip()# 运行自适应反馈循环adaptive_feedback_loop(code_snippet)
import openai
# 设置OpenAI API密钥
openai.api_key = "your-api-key"
# 目标代码片段
code_snippet = """
import os
def execute_command(user_input):
    os.system(user_input)
def run_safe_command():
    command = 'ls'
    os.system(command)
"""
# 使用LLM对代码进行初步审计
def audit_code_with_llm(code):
    prompt = f"""
    请分析以下Python代码，判断是否存在远程代码执行（RCE）漏洞：
    {code}
    需要你：
    1. 明确代码中不安全的部分，并说明其风险。
    2. 提供相应的修复建议。
    """
    
    response = openai.Completion.create(
        model="code-davinci-002",
        prompt=prompt,
        max_tokens=300,
        temperature=0.5
    )
    return response.choices[0].text.strip()
# 分析代码
def get_rce_vulnerability_feedback(code):
    result = audit_code_with_llm(code)
    print("初步审计结果：")
    print(result)
    
    if "RCE漏洞" in result or "命令执行" in result:
        return True, result
    return False, result
# 修复漏洞并重新审计代码
def fix_code_and_reaudit(code, fix_suggestions):
    # 使用LLM根据修复建议修改代码
    prompt = f"""
    根据以下修复建议，请修改代码并提供修改后的版本：
    修复建议：{fix_suggestions}
    目标代码：{code}
    """
    response = openai.Completion.create(
        model="code-davinci-002",
        prompt=prompt,
        max_tokens=300,
        temperature=0.7
    )
    fixed_code = response.choices[0].text.strip()
    print("修复后的代码：")
    print(fixed_code)
    
    # 再次进行代码审计
    is_fixed, audit_feedback = get_rce_vulnerability_feedback(fixed_code)
    
    return is_fixed, fixed_code, audit_feedback
# 自适应反馈机制
def adaptive_feedback_loop(code):
    is_fixed, feedback = get_rce_vulnerability_feedback(code)
    
    attempt = 1
    max_attempts = 5
    while not is_fixed and attempt <= max_attempts:
        print(f"\n[尝试第 {attempt} 次] 修复并审计代码")
        # 提取修复建议
        fix_suggestions = extract_fix_suggestions(feedback)
        
        # 修复代码并审计
        is_fixed, fixed_code, audit_feedback = fix_code_and_reaudit(code, fix_suggestions)
        
        if is_fixed:
            print("漏洞已修复！")
            break
        
        # 更新反馈
        feedback = audit_feedback
        attempt += 1
    
    if not is_fixed:
        print("无法修复漏洞，请检查修复建议。")
    else:
        print("最终修复后的代码：")
        print(fixed_code)
# 提取修复建议（示例）
def extract_fix_suggestions(feedback):
    prompt = f"""
    以下是LLM的审计反馈，提取其中的修复建议：
    {feedback}
    请简要列出修复步骤。
    """
    response = openai.Completion.create(
        model="code-davinci-002",
        prompt=prompt,
        max_tokens=150,
        temperature=0.6
    )
    return response.choices[0].text.strip()
# 运行自适应反馈循环
adaptive_feedback_loop(code_snippet)
```  
  
#### 代码逻辑分析  
####   
  
1. **初步审计与漏洞检测**  
  
首先，我们通过get_rce_vulnerability_feedback  
函数对代码进行初步审计。如果LLM分析到存在潜在的RCE漏洞或不安全的命令执行调用，反馈将被用来生成修复建议。  
  
2. **自适应反馈机制**  
  
在初次审计后，adaptive_feedback_loop  
函数会启动自适应反馈机制。如果漏洞未能修复，LLM根据修复建议对代码进行修改，并再次进行审计。这个过程会持续进行多次，直到漏洞被彻底修复，或者达到最大尝试次数。  
  
3. **修复代码并重新审计**  
  
fix_code_and_reaudit  
函数接收LLM给出的修复建议，修改代码后再次审计，确保修复有效。如果反馈仍然指示有漏洞存在，过程会继续。  
  
4. **提取修复建议**  
  
在每一轮反馈中，extract_fix_suggestions  
函数提取LLM给出的修复步骤，帮助开发者理解如何修复漏洞，并确保后续代码不会再有类似问题。  
### 示例：自适应修复流程  
  
假设目标代码存在RCE漏洞，LLM通过自适应反馈循环提供了以下修复过程：  
#### 初步审计结果：  
  
```
审计结果：存在远程代码执行（RCE）漏洞。函数`execute_command`中使用了`os.system`直接执行用户输入的命令，攻击者可以利用此漏洞执行任意命令。修复建议：使用`subprocess.run`替代`os.system`，并进行严格的输入验证。
审计结果：
存在远程代码执行（RCE）漏洞。函数`execute_command`中使用了`os.system`直接执行用户输入的命令，攻击者可以利用此漏洞执行任意命令。
修复建议：使用`subprocess.run`替代`os.system`，并进行严格的输入验证。
```  
  
#### 自适应修复步骤：  
1. **第1轮修复**  
：  
  
1. LLM提供了修复代码：将os.system(user_input)  
替换为subprocess.run(user_input)  
，并添加输入验证。  
  
1. 反馈：修复代码依然存在漏洞，提示需要增加输入过滤。  
  
1. **第2轮修复**  
：  
  
1. 根据反馈，LLM提供了更严格的输入过滤方案，如白名单机制，仅允许特定命令执行。  
  
1. 反馈：漏洞已修复，代码没有引入新漏洞。  
  
#### 最终修复后的代码：  
  
```
import subprocessdef execute_command(user_input):    allowed_commands = ["ls", "pwd"]    if user_input in allowed_commands:        subprocess.run([user_input], shell=True)    else:        print("非法命令！")
import subprocess
def execute_command(user_input):
    allowed_commands = ["ls", "pwd"]
    if user_input in allowed_commands:
        subprocess.run([user_input], shell=True)
    else:
        print("非法命令！")
```  
  
  
  
  
  
  
  
