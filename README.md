# Older-management-system
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>养老院系统 - 老人信息录入</title>
    <style>
        .error {
            color: red;
            font-size: 12px;
        }
    </style>
</head>
<body>
    <form id="elderInfoForm">
        <label for="name">姓名:</label>
        <input type="text" id="name" name="name" maxlength="50">
        <span id="nameError" class="error"></span><br><br>

        <label for="age">年龄:</label>
        <input type="text" id="age" name="age">
        <span id="ageError" class="error"></span><br><br>

        <label for="gender">性别:</label>
        <select id="gender" name="gender">
            <option value="">请选择</option>
            <option value="male">男</option>
            <option value="female">女</option>
        </select>
        <span id="genderError" class="error"></span><br><br>

        <label for="healthStatus">健康状况:</label>
        <textarea id="healthStatus" name="healthStatus" rows="4" cols="50"></textarea>
        <span id="healthStatusError" class="error"></span><br><br>

        <label for="contact">联系方式:</label>
        <input type="text" id="contact" name="contact">
        <span id="contactError" class="error"></span><br><br>

        <button type="submit">提交</button>
    </form>

    <script>
        // 获取表单元素
        const form = document.getElementById('elderInfoForm');
        const nameInput = document.getElementById('name');
        const ageInput = document.getElementById('age');
        const genderInput = document.getElementById('gender');
        const healthStatusInput = document.getElementById('healthStatus');
        const contactInput = document.getElementById('contact');

        // 错误信息显示元素
        const nameError = document.getElementById('nameError');
        const ageError = document.getElementById('ageError');
        const genderError = document.getElementById('genderError');
        const healthStatusError = document.getElementById('healthStatusError');
        const contactError = document.getElementById('contactError');

        // 提交事件监听器
        form.addEventListener('submit', (event) => {
            event.preventDefault(); // 阻止默认提交
            // 表单验证
            const isValid = validateForm();
            if (isValid) {
                // 表单验证通过，提交表单（这里可以改为发送Ajax请求或其他处理）
                alert('老人信息提交成功');
                // form.submit(); // 如果需要实际提交表单，可以取消注释这行代码
            }
        });

        // 表单验证函数
        function validateForm() {
            let isValid = true;

            // 验证姓名（必填，最多 50 字符）
            if (!nameInput.value.trim()) {
                showError(nameInput, nameError, '姓名不能为空');
                isValid = false;
            } else if (nameInput.value.length > 50) {
                showError(nameInput, nameError, '姓名不能超过 50 个字符');
                isValid = false;
            } else {
                clearError(nameError);
            }

            // 验证年龄（必填，正整数）
            if (!ageInput.value.trim() || isNaN(ageInput.value) || ageInput.value < 0) {
                showError(ageInput, ageError, '年龄必须为正整数');
                isValid = false;
            } else {
                clearError(ageError);
            }

            // 验证性别（必填）
            if (!genderInput.value) {
                showError(genderInput, genderError, '性别不能为空');
                isValid = false;
            } else {
                clearError(genderError);
            }

            // 验证健康状况（必填）
            if (!healthStatusInput.value.trim()) {
                showError(healthStatusInput, healthStatusError, '健康状况不能为空');
                isValid = false;
            } else {
                clearError(healthStatusError);
            }

            // 验证联系方式（必填，包含数字和可能的特殊字符，如空格、破折号等）
            const contactRegex = /^[0-9\s\-]+$/; // 简单的正则表达式，允许数字、空格和破折号
            if (!contactInput.value.trim() || !contactRegex.test(contactInput.value)) {
                showError(contactInput, contactError, '联系方式必须包含有效的数字和可能的特殊字符（如空格、破折号）');
                isValid = false;
            } else {
                clearError(contactError);
            }

            return isValid;
        }

        // 显示错误信息函数
        function showError(inputElement, errorElement, errorMessage) {
            inputElement.style.borderColor = 'red';
            errorElement.textContent = errorMessage;
        }

        // 清除错误信息函数
        function clearError(errorElement) {
            const inputElement = errorElement.previousElementSibling;
            inputElement.style.borderColor = '';
            errorElement.textContent = '';
        }
    </script>
</body>
</html>
