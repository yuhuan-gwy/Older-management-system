# Older-management-system
// 获取表单元素const form = document.getElementById('elderlyForm');
const nameInput = document.getElementById('name');
const ageInput = document.getElementById('age');
const genderInput = document.getElementById('gender');
const contactPersonNameInput = document.getElementById('contactPersonName');
const contactPhoneInput = document.getElementById('contactPhone');
const roomInput = document.getElementById('room');
const medicalConditionInput = document.getElementById('medicalCondition');
const fileInput = document.getElementById('healthDoc');
// 提交事件监听器
form.addEventListener('submit', (event) => {
  event.preventDefault(); // 阻止默认提交
  // 表单验证
  const isValid = validateForm();
  if (isValid) {
    // 表单验证通过，提交表单
    alert('老人信息提交成功');
    form.submit(); // 提交表单
  }
});
// 表单验证函数
function validateForm() {
  let isValid = true;
  // 验证老人姓名（必填）
  if (!nameInput.value.trim()) {
    showError(nameInput, '老人姓名不能为空');
    isValid = false;
  } else {
    clearError(nameInput);
  }
  // 验证年龄（必填，数字，合理范围）
  const ageValue = parseInt(ageInput.value.trim(), 10);
  if (isNaN(ageValue) || ageValue <= 0 || ageValue > 120) {
    showError(ageInput, '年龄必须为有效的数字且在0到120之间');
    isValid = false;
  } else {
    clearError(ageInput);
  }
  // 验证性别（必选）
  if (!genderInput.value.trim()) {
    showError(genderInput, '请选择性别');
    isValid = false;
  } else {
    clearError(genderInput);
  }
  // 验证紧急联系人姓名（必填）
  if (!contactPersonNameInput.value.trim()) {
    showError(contactPersonNameInput, '紧急联系人姓名不能为空');
    isValid = false;
  } else {
    clearError(contactPersonNameInput);
  }
  // 验证紧急联系人电话（必填，有效的电话号码格式）
  const phonePattern = /^[0-9]{10,15}$/; // 简单的电话号码验证，可根据需要调整
  if (!contactPhoneInput.value.trim().match(phonePattern)) {
    showError(contactPhoneInput, '请输入有效的电话号码');
    isValid = false;
  } else {
    clearError(contactPhoneInput);
  }
  // 验证房间号（必填）
  if (!roomInput.value.trim()) {
    showError(roomInput, '房间号不能为空');
    isValid = false;
  } else {
    clearError(roomInput);
  }
// 验证健康状况（可选，但如果有填写，不能为空）
  if (medicalConditionInput.value.trim()) {
    if (!medicalConditionInput.value.trim()) {
      showError(medicalConditionInput, '健康状况不能为空（如果填写）');
      isValid = false;
    } else {
      clearError(medicalConditionInput);
    }
  } else {
    clearError(medicalConditionInput); // 如果没有填写，也清除可能的错误
  }
  // 验证健康文档（PDF 格式，最大 5MB）
  if (fileInput.files.length > 0) {
    const file = fileInput.files[0];
    const validExtensions = ['application/pdf'];
    const maxSize = 5 * 1024 * 1024; // 5MB
    if (!validExtensions.includes(file.type)) {
      showError(fileInput, '健康文档必须为 PDF 格式');
      isValid = false;
    } else if (file.size > maxSize) {
      showError(fileInput, '健康文档不能超过 5MB');
      isValid = false;
    } else {
      clearError(fileInput);
    }
  } else {
    // 如果不需要文档作为必填项，可以注释掉以下错误显示
    // showError(fileInput, '请上传健康文档');
    // isValid = false; // 如果文档是必填项，则取消注释这两行
  }

  return isValid;
}
// 显示错误信息的函数
function showError(input, message) {
  const formGroup = input.parentElement;
  const errorMessage = document.createElement('div');
  errorMessage.textContent = message;
  errorMessage.className = 'error';
  formGroup.appendChild(errorMessage);
}
// 清除错误信息的函数
function clearError(input) {
  const formGroup = input.parentElement;
  const errorMessage = formGroup.querySelector('.error');
  if (errorMessage) {
    formGroup.removeChild(errorMessage);
  }
}
