# 开发手册
* 开发人员：郑铭杰 余宏城
* 单位：重庆大学
* 联系方式：1072200141@qq.com
## 引言
欢迎您使用本开发手册。本手册的目的是为开发团队提供一份全面的指南，以支持项目的开发、维护和交付。无手，本手册都将为您提供必需的信息和资源，帮助您更高效地完成任务。
## 手册目的
本手册旨在：

- **规范性**：提供统一的开发流程、标准和最佳实践，以确保代码质量和一致性。
- **协作性**：促进团队内部的沟通与协作，确保项目目标和进度的透明度。

## 项目概述 
#### 项目描述
根据"信息安全导论"课程第8-9次课讲述的AES算法，在课外认真阅读教科书附录D的内容，学习了解S-AES算法，来编程实现加、解密程序。

#### 项目功能：
* 提供GUI解密支持用户交互
* 对相同的密文进行交叉测试
* 把ASII编码字符串作为加密算法的数据进行测试
* 将S-AES算法通过双重加密进行扩展
* 使用相同密钥的明、密文对，使用中间相遇攻击的方法找到正确的密钥Key
* 将S-AES算法通过三重加密进行扩展
* 基于S-AES算法，使用密码分组链(CBC)模式对较长的明文消息进行加密

## 开发环境
* 操作系统：Windows11
* 版本控制：Github
* 语言&框架：HTML5，CSS3，JavaScript
* Web浏览器：Microsoft Edge
* 编辑器语言：markdown
* 集成开发环境：Visual Studio Code

## 开发流程
### 函数功能解释
#### 1. `openTab(evt, tabName)`
**功能**: 实现标签页的切换功能。


#### 2. `OR_8(a, b)`
**功能**: 对两个 8 位数组进行按位异或（XOR）操作。
- **参数**:
  - `a`: 第一个 8 位数组。
  - `b`: 第二个 8 位数组。
- **返回值**: 结果数组，包含每个位的异或结果。

#### 3. `OR_4(a, b)`
**功能**: 对两个 4 位数组进行按位异或（XOR）操作。
- **参数**:
  - `a`: 第一个 4 位数组。
  - `b`: 第二个 4 位数组。
- **返回值**: 结果数组，包含每个位的异或结果。

#### 4. `SubBytes(temp)`
**功能**: 使用 S-Box 和替换表对输入的 8 位数组进行字节替换。
- **参数**:
  - `temp`: 一个包含 8 位的数组。
#### 5. `g(temp, rcon)`
**功能**: 密钥扩展中的一个辅助函数，包含旋转、替换和与常量异或操作。
- **参数**:
  - `temp`: 一个包含 8 位的数组。
  - `rcon`: 一个常量数组，用于与替换后的数组进行异或。
- **返回值**: 经过处理后的 8 位数组。

#### 6. `AddRoundKey(mingwen, key)`
**功能**: 将当前明文与轮密钥进行按位异或，完成“添加轮密钥”步骤。
- **参数**:
  - `mingwen`: 2x8 数组，表示当前的明文状态。
  - `key`: 2x8 数组，表示当前的轮密钥。
- **描述**:
  - 遍历 `mingwen` 和 `key` 数组，对每个位进行异或操作，更新 `mingwen` 数组。

#### 7. `ShiftRows(temp)`
**功能**: 对明文的行进行移位操作。
- **参数**:
  - `temp`: 2x8 矩阵，表示当前的明文状态。

#### 8. `x_fx(last, res)`
**功能**: 实现有限域上的多项式乘法，用于 `multiply` 函数中的乘法操作。
- **参数**:
  - `last`: 一个包含 4 位的数组，表示多项式的系数。
  - `res`: 一个包含 4 位的数组，用于存储结果。

#### 9. `multiply(a, b)`
**功能**: 在有限域 GF(2^4) 上对两个 4 位多项式进行乘法运算。
- **参数**:
  - `a`: 第一个 4 位数组，表示多项式系数。
  - `b`: 第二个 4 位数组，表示多项式系数。
- **返回值**: 结果数组，包含乘法后的 4 位结果。

#### 10. `MixColumns(mingwen)`
**功能**: 对明文的列进行混合操作，以增加加密的复杂性。
- **参数**:
  - `mingwen`: 2x8 矩阵，表示当前的明文状态。

#### 11. `keyExpansion(key)`
**功能**: 扩展初始密钥，生成用于各轮加密的轮密钥。
- **参数**:
  - `key`: 2x8 数组，表示初始密钥。
- **返回值**: 包含两个轮密钥的数组 `[key1, key2]`。
- **描述**:
  - 使用函数 `g` 处理当前密钥，生成第一个轮密钥 `key1`。
  - 再次使用 `g` 处理 `key1`，生成第二个轮密钥 `key2`。
  - 返回生成的轮密钥数组。

#### 12. `encrypt(plaintext, key)`
**功能**: 执行加密过程，将明文转换为密文。
- **参数**:
  - `plaintext`: 16 位数组，表示明文。
  - `key`: 16 位数组，表示密钥。
- **返回值**: 16 位数组，表示密文。
- **描述**:
  - 将明文和密钥分别转换为 2x8 的矩阵形式。
  - 执行密钥扩展，生成轮密钥 `key1` 和 `key2`。
  - **Round 0**: 初始轮，执行 `AddRoundKey`。
  - **Round 1**:
    - 执行 `SubBytes` 替换。
    - 执行 `ShiftRows` 行移位。
    - 执行 `MixColumns` 列混合。
    - 执行 `AddRoundKey` 添加轮密钥 `key1`。
  - **Round 2**:
    - 执行 `SubBytes` 替换。
    - 执行 `ShiftRows` 行移位。
    - 执行 `AddRoundKey` 添加轮密钥 `key2`。
  - 将加密后的矩阵重新组合成 16 位密文数组并返回。

#### 13. `asciiToBinary(str)`
**功能**: 将 ASCII 字符串转换为二进制数组。
- **参数**:
  - `str`: 输入字符串，必须为 2 个 ASCII 字符。
- **返回值**: 16 位二进制数组，或在输入不符合要求时返回 `null`。
- **描述**:
  - 检查输入字符串长度是否为 2。
  - 将每个字符转换为 8 位二进制表示，拼接成 16 位字符串。
  - 将二进制字符串拆分为数组，并转换为整数数组。

#### 14. `binaryToASCII(binArray)`
**功能**: 将 16 位二进制数组转换为 ASCII 字符串。
- **参数**:
  - `binArray`: 16 位二进制数组。
- **返回值**: 转换后的 2 个 ASCII 字符串，或空字符串（如果输入长度不符）。
- **描述**:
  - 检查输入数组长度是否为 16。
  - 将前 8 位转换为第一个字符，后 8 位转换为第二个字符。
  - 拼接两个字符并返回。

#### 15. `basicEncrypt()`
**功能**: 执行基本的二进制字符串加密。
- **参数**: 无（从 HTML 输入元素获取数据）。
- **描述**:
  - 获取用户输入的 16 位二进制明文和密钥。
  - 验证输入格式和长度。
  - 将输入字符串转换为整数数组。
  - 调用 `encrypt` 函数进行加密。
  - 将密文结果显示在 HTML 页面上。

#### 16. `asciiEncrypt()`
**功能**: 执行 ASCII 字符串加密。
- **参数**: 无（从 HTML 输入元素获取数据）。
- **描述**:
  - 获取用户输入的 ASCII 明文和 16 位二进制密钥。
  - 验证密钥的格式和长度。
  - 将 ASCII 明文转换为二进制数组。
  - 调用 `encrypt` 函数进行加密。
  - 将密文二进制结果转换回 ASCII 字符串，并显示在 HTML 页面上。

#### 17. `doubleEncrypt()`
**功能**: 执行双重加密过程。
- **参数**: 无（从 HTML 输入元素获取数据）。
- **描述**:
  - 获取用户输入的 16 位二进制明文和两个 16 位二进制密钥。
  - 验证输入格式和长度。
  - 将输入字符串转换为整数数组。
  - 使用第一个密钥对明文进行加密，得到中间密文。
  - 使用第二个密钥对中间密文进行加密，得到最终密文。
  - 将最终密文结果显示在 HTML 页面上。

#### 18. `tripleEncrypt()`
**功能**: 执行三重加密过程。
- **参数**: 无（从 HTML 输入元素获取数据）。
- **描述**:
  - 获取用户输入的 16 位二进制明文和三个 16 位二进制密钥。
  - 验证输入格式和长度。
  - 将输入字符串转换为整数数组。
  - 使用第一个密钥对明文进行加密，得到第一次加密的密文。
  - 使用第二个密钥对第一次加密的密文进行加密，得到第二次加密的密文。
  - 使用第三个密钥对第二次加密的密文进行加密，得到最终密文。
  - 将最终密文结果显示在 HTML 页面上。

---

### 其他重要变量说明

#### 1. `sBox`
**描述**: S-Box（替代盒），用于非线性字节替换操作，增强加密算法的安全性。

```javascript
const sBox = [
    [9, 4, 10, 11],
    [13, 1, 8, 5],
    [6, 2, 0, 3],
    [12, 14, 15, 7]
];
```

#### 2. `Replace`
**描述**: 替换表，用于将 S-Box 输出的数值进一步映射为 4 位二进制数。

```javascript
const Replace = [
    [0,0,0,0],
    [0,0,0,1],
    [0,0,1,0],
    [0,0,1,1],
    [0,1,0,0],
    [0,1,0,1],
    [0,1,1,0],
    [0,1,1,1],
    [1,0,0,0],
    [1,0,0,1],
    [1,0,1,0],
    [1,0,1,1],
    [1,1,0,0],
    [1,1,0,1],
    [1,1,1,0],
    [1,1,1,1]
];
```

#### 3. `rcon1` 和 `rcon2`
**描述**: Rcon（轮常量），在密钥扩展过程中用于与替换后的密钥进行异或操作，增强密钥的随机性。

```javascript
const rcon1 = [1,0,0,0,0,0,0,0];
const rcon2 = [0,0,1,1,0,0,0,0];
```

---

### 主要函数内容

#### 基础加密函数
```
 function basicEncrypt() {
        let plaintextStr = document.getElementById('basic-plaintext')value.trim();
        let keyStr = document.getElementById('basic-key').value.trim();

        if(plaintextStr.length !==16 || !/^[01]+$/.test(plaintextStr)){
            alert("Plaintext must be a 16-bit binary string.");
            return;
        }
        if(keyStr.length !==16 || !/^[01]+$/.test(keyStr)){
            alert("Key must be a 16-bit binary string.");
            return;
        }

        let plaintext = plaintextStr.split('').map(bit => parseInt(bit));
        let key = keyStr.split('').map(bit => parseInt(bit));

        let ciphertext = encrypt(plaintext, key);
        let ciphertextStr = ciphertext.join('');
        document.getElementById('basic-output').innerHTML = `<strong>密文:</strong> ${ciphertextStr}`;
    }
```
#### ASCII码加密
```
function asciiEncrypt() {
        let plaintextStr = document.getElementById('ascii-plaintext').value;
        let keyStr = document.getElementById('ascii-key').value.trim();

        if(keyStr.length !==16 || !/^[01]+$/.test(keyStr)){
            alert("Key must be a 16-bit binary string.");
            return;
        }

        let plaintext = asciiToBinary(plaintextStr);
        if(!plaintext) return;

        let key = keyStr.split('').map(bit => parseInt(bit));

        let ciphertext = encrypt(plaintext, key);
        let ciphertextStr = binaryToASCII(ciphertext);
        document.getElementById('ascii-output').innerHTML = `<strong>密文:</strong> ${ciphertextStr}`;
    }

``` 
#### 双重加密函数
```
function doubleEncrypt() {
        let plaintextStr = document.getElementById('double-plaintext').value.trim();
        let key1Str = document.getElementById('double-key1').value.trim();
        let key2Str = document.getElementById('double-key2').value.trim();

        if(plaintextStr.length !==16 || !/^[01]+$/.test(plaintextStr)){
            alert("Plaintext must be a 16-bit binary string.");
            return;
        }
        if(key1Str.length !==16 || !/^[01]+$/.test(key1Str)){
            alert("Key 1 must be a 16-bit binary string.");
            return;
        }
        if(key2Str.length !==16 || !/^[01]+$/.test(key2Str)){
            alert("Key 2 must be a 16-bit binary string.");
            return;
        }

        let plaintext = plaintextStr.split('').map(bit => parseInt(bit));
        let key1 = key1Str.split('').map(bit => parseInt(bit));
        let key2 = key2Str.split('').map(bit => parseInt(bit));

        // First Encryption
        let firstCipher = encrypt(plaintext, key1);
        // Second Encryption
        let finalCipher = encrypt(firstCipher, key2);

        let ciphertextStr = finalCipher.join('');
        document.getElementById('double-output').innerHTML = `<strong>密文:</strong> ${ciphertextStr}`;
    }
```
#### 三重加密函数
```
function tripleEncrypt() {
        let plaintextStr = document.getElementById('triple-plaintext').value.trim();
        let key1Str = document.getElementById('triple-key1').value.trim();
        let key2Str = document.getElementById('triple-key2').value.trim();
        let key3Str = document.getElementById('triple-key3').value.trim();

        if(plaintextStr.length !==16 || !/^[01]+$/.test(plaintextStr)){
            alert("Plaintext must be a 16-bit binary string.");
            return;
        }
        if(key1Str.length !==16 || !/^[01]+$/.test(key1Str)){
            alert("Key 1 must be a 16-bit binary string.");
            return;
        }
        if(key2Str.length !==16 || !/^[01]+$/.test(key2Str)){
            alert("Key 2 must be a 16-bit binary string.");
            return;
        }
        if(key3Str.length !==16 || !/^[01]+$/.test(key3Str)){
            alert("Key 3 must be a 16-bit binary string.");
            return;
        }

        let plaintext = plaintextStr.split('').map(bit => parseInt(bit));
        let key1 = key1Str.split('').map(bit => parseInt(bit));
        let key2 = key2Str.split('').map(bit => parseInt(bit));
        let key3 = key3Str.split('').map(bit => parseInt(bit));

        // First Encryption
        let firstCipher = encrypt(plaintext, key1);
        // Second Encryption
        let secondCipher = encrypt(firstCipher, key2);
        // Third Encryption
        let finalCipher = encrypt(secondCipher, key3);

        let ciphertextStr = finalCipher.join('');
        document.getElementById('triple-output').innerHTML = `<strong>密文:</strong> ${ciphertextStr}`;
    }
```
#### 中间相遇攻击函数
```
function middleAttack() {
            const P1 = document.getElementById('attack-plaintext1').value.trim();
            const C1 = document.getElementById('attack-ciphertext1').value.trim();
            const P2 = document.getElementById('attack-plaintext2').value.trim();
            const C2 = document.getElementById('attack-ciphertext2').value.trim();
            const attackOutputDiv = document.getElementById('attack-output');
            attackOutputDiv.innerHTML = '';

            // 输入类型假设为二进制
            const inputType = 'binary';

            try {
                // 验证输入格式
                const binaryRegex = /^[01]{16}$/;
                if (!binaryRegex.test(P1)) {
                    throw new Error("输入明文1必须是16位二进制。");
                }
                if (!binaryRegex.test(C1)) {
                    throw new Error("输入密文1必须是16位二进制。");
                }
                if (!binaryRegex.test(P2)) {
                    throw new Error("输入明文2必须是16位二进制。");
                }
                if (!binaryRegex.test(C2)) {
                    throw new Error("输入密文2必须是16位二进制。");
                }

                // 双重加密：C = E(K2, E(K1, P))
                // 目标：找到 K1 和 K2，使得：
                // C1 = E(K2, E(K1, P1))
                // C2 = E(K2, E(K1, P2))

                // 中间相遇攻击步骤：
                // 1. 遍历所有可能的 K1，计算 E(K1, P1) 和 E(K1, P2)，存储中间结果。
                // 2. 遍历所有可能的 K2，计算 D(K2, C1) 和 D(K2, C2)，检查是否有匹配的中间结果。
                // 3. 如果 E(K1, P1) = D(K2, C1) 且 E(K1, P2) = D(K2, C2)，则找到有效的 K1 和 K2。

                const startTime = performance.now();

                // 第一阶段：构建 K1 -> (E(K1, P1), E(K1, P2)) 的映射
                let K1Map = {};
                for (let K1 = 0; K1 <= 0xFFFF; K1++) {
                    let K1Str = K1.toString(2).padStart(16, '0');
                    let E_P1 = encrypt(P1, K1Str);
                    let E_P2 = encrypt(P2, K1Str);
                    K1Map[K1Str] = { E_P1, E_P2 };
                }

                // 第二阶段：遍历所有可能的 K2，计算 D(K2, C1) 和 D(K2, C2)，并寻找匹配的 K1
                let foundKeys = [];

                for (let K2 = 0; K2 <= 0xFFFF; K2++) {
                    let K2Str = K2.toString(2).padStart(16, '0');
                    let D_C1 = decrypt(C1, K2Str);
                    let D_C2 = decrypt(C2, K2Str);

                    // 遍历所有 K1，检查是否存在 K1 使得 E(K1, P1) = D_C1 且 E(K1, P2) = D_C2
                    for (let [K1Str, intermediates] of Object.entries(K1Map)) {
                        if (intermediates.E_P1 === D_C1 && intermediates.E_P2 === D_C2) {
                            foundKeys.push({ K1: K1Str, K2: K2Str });
                        }
                    }
                }

                const endTime = performance.now();
                const elapsedTime = ((endTime - startTime) / 1000).toFixed(6);

                // 处理结果
                if (foundKeys.length > 0) {
                    let uniqueKeys = [];
                    // 去除重复的 K1 和 K2 组合
                    foundKeys.forEach(pair => {
                        // 确保每组 K1 和 K2 唯一
                        if (!uniqueKeys.some(existing => existing.K1 === pair.K1 && existing.K2 === pair.K2)) {
                            uniqueKeys.push(pair);
                        }
                    });

                    let resultHTML = `找到 ${uniqueKeys.length} 组匹配的密钥对:<br>`;
                    uniqueKeys.forEach((pair, index) => {
                        resultHTML += `组 ${index + 1}: K1 = ${pair.K1}, K2 = ${pair.K2}<br>`;
                    });
                    resultHTML += `算法执行时间: ${elapsedTime} 秒`;
                    attackOutputDiv.innerHTML = `<span class="info">${resultHTML}</span>`;
                } else {
                    attackOutputDiv.innerHTML = `<span class="error">未找到匹配的密钥对</span>`;
                }

            } catch (error) {
                attackOutputDiv.innerHTML = `<span class="error">${error.message}</span>`;
            }
        }
```
#### CBC加密函数
```
function encryptText() {
            const plaintext = document.getElementById('plaintext').value.trim();
            const key = document.getElementById('key').value.trim();
            const mode = document.getElementById('mode').value;
            const iv = document.getElementById('iv').value.trim();
            const outputDiv = document.getElementById('output');
            outputDiv.innerHTML = '';

            // 简单假设输入类型为二进制
            const inputType = 'binary';

            try {
                if (!/^[01]{16}$/.test(key)) {
                    throw new Error("密钥必须是16位二进制。");
                }

                if (mode === 'CBC') {
                    if (!/^[01]{16}$/.test(iv)) {
                        throw new Error("初始向量 IV 必须是16位二进制。");
                    }
                    if (plaintext.length === 0 || plaintext.length % 16 !== 0) {
                        throw new Error("明文长度必须是16的倍数。");
                    }
                    const ciphertext = encryptCBC(plaintext, key, iv, inputType);
                    outputDiv.innerHTML = `密文：${ciphertext}`;
                } else {
                    // ECB模式
                    if (plaintext.length === 0 || plaintext.length % 16 !== 0) {
                        throw new Error("明文长度必须是16的倍数。");
                    }
                    let blocks = preprocessInput(plaintext, inputType);
                    let ciphertext = blocks.map(block => encrypt(block, key)).join('');
                    outputDiv.innerHTML = `密文：${ciphertext}`;
                }
            } catch (error) {
                outputDiv.innerHTML = `<span class="error">${error.message}</span>`;
            }
        }
```
#### CBC解密函数
```
function decryptText() {
            const ciphertext = document.getElementById('plaintext').value.trim();
            const key = document.getElementById('key').value.trim();
            const mode = document.getElementById('mode').value;
            const iv = document.getElementById('iv').value.trim();
            const outputDiv = document.getElementById('output');
            outputDiv.innerHTML = '';

            // 简单假设输入类型为二进制
            const inputType = 'binary';

            try {
                if (!/^[01]{16}$/.test(key)) {
                    throw new Error("密钥必须是16位二进制。");
                }

                if (mode === 'CBC') {
                    if (!/^[01]{16}$/.test(iv)) {
                        throw new Error("初始向量 IV 必须是16位二进制。");
                    }
                    if (ciphertext.length === 0 || ciphertext.length % 16 !== 0) {
                        throw new Error("密文长度必须是16的倍数。");
                    }
                    const plaintext = decryptCBC(ciphertext, key, iv, inputType);
                    outputDiv.innerHTML = `明文：${plaintext}`;
                } else {
                    // ECB模式
                    if (ciphertext.length === 0 || ciphertext.length % 16 !== 0) {
                        throw new Error("密文长度必须是16的倍数。");
                    }
                    let blocks = preprocessInput(ciphertext, inputType);
                    let plaintext = blocks.map(block => decrypt(block, key)).join('');
                    outputDiv.innerHTML = `明文：${plaintext}`;
                }
            } catch (error) {
                outputDiv.innerHTML = `<span class="error">${error.message}</span>`;
            }
        }
```

## 结束语
感谢您花时间阅读本开发手册。希望您能够从中获得需要的信息和指导，并成功地运用我们提供的功能与工具。我们的目标是为开发者提供一份清晰且有用的参考文档，使所有使用者能够高效地完成项目，并应对各种开发挑战。

如果您在开发过程中遇到任何问题或需要进一步的指导，请不要犹豫与我们联系。我们的团队始终愿意协助您解决问题，回复反馈，持续改进文档的质量与适用性。

我们期待看到您在项目中取得的成功，并希望您能将使用体验与我们分享。同时，请定期查看我们的更新与新功能，以保持您的知识与技术的更新。

再次感谢您的支持与参与，祝您开发顺利，取得丰硕的成果！

---

请根据项目的具体情况和团队文化调整以上内容，使其更加符合您所需的语气和风格。如果需要更多帮助，请随时告知！
