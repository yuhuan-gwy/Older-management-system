# Older-management-system
// 获取表单元素
const form = document.getElementById('activityForm');
const activityNameInput = document.getElementById('activityName');
const activityDateInput = document.getElementById('activityDate');
const activityPurposeInput = document.getElementById('activityPurpose');
const activityContentInput = document.getElementById('activityContent');
const fileInput = document.getElementById('attachment');

// 提交事件监听器
form.addEventListener('submit', (event) => {
  event.preventDefault(); // 阻止默认提交

  // 表单验证
  const isValid = validateForm();

  if (isValid) {
    // 表单验证通过，提交表单
    alert('活动安排表单提交成功');
    form.submit(); // 提交表单
  }
});

// 表单验证函数
function validateForm() {
  let isValid = true;

  // 验证活动名称（必填，最多 100 字符）
  if (!activityNameInput.value.trim()) {
    showError(activityNameInput, '活动名称不能为空');
    isValid = false;
  } else if (activityNameInput.value.length > 100) {
    showError(activityNameInput, '活动名称不能超过 100 个字符');
    isValid = false;
  } else {
    clearError(activityNameInput);
  }

  // 验证活动日期（必填，必须为有效日期）
  if (!activityDateInput.value.trim()) {
    showError(activityDateInput, '活动日期不能为空');
    isValid = false;
  } else {
    const selectedDate = new Date(activityDateInput.value);
    const today = new Date();
    if (selectedDate < today) {
      showError(activityDateInput, '活动日期必须是今天或未来的日期');
      isValid = false;
    } else {
      clearError(activityDateInput);
    }
  }

  // 验证活动目的（必填）
  if (!activityPurposeInput.value.trim()) {
    showError(activityPurposeInput, '活动目的不能为空');
    isValid = false;
  } else {
    clearError(activityPurposeInput);
  }

  // 验证活动内容（必填）
  if (!activityContentInput.value.trim()) {
    showError(activityContentInput, '活动内容不能为空');
    isValid = false;
  } else {
    clearError(activityContentInput);
  }

  // 验证附件（PDF 格式，最大 10MB）
  if (fileInput.files.length > 0) {
    const file = fileInput.files[0];
    const validExtensions = ['application/pdf'];
    const maxSize = 10 * 1024 * 1024; // 10MB

    if (!validExtensions.includes(file.type)) {
      showError(fileInput, '附件必须为 PDF 格式');
      isValid = false;
    } else if (file.size > maxSize) {
      showError(fileInput, '附件不能超过 10MB');
      isValid = false;
    } else {
      clearError(fileInput);
    }
  } else {
    showError(fileInput, '请上传附件');
    isValid = false;
  }

  return isValid;
}

// 显示错误信息
function showError(input, message) {
  const errorElement = input.nextElementSibling;
  errorElement.textContent = message;
  errorElement.classList.add('error');
}

// 清除错误信息
function clearError(input) {
  const errorElement = input.nextElementSibling;
  errorElement.textContent = '';
  errorElement.classList.remove('error');
}
