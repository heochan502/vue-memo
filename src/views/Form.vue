<script setup>
import { reactive, onMounted } from 'vue';
import { HttpService } from '@/services/HttpService';
import { useRoute, useRouter } from 'vue-router';
// 스토리지 서비스의 객체(인스턴스) 생성
//p.175
//메모 등록, 메모 디테일, 메모 수정
const httpService = new HttpService();
// 라우터 객체
const router = useRouter();
const route = useRoute();
// 반응형 상태
const state = reactive({
  memo: {
    id: 0,
    title: '',
    content: '',
    createdAt: '',
  },
});
// 데이터 제출
const submit = () => {
  if (route.params.id) {
    httpService.setItem(state.memo);
  } else {
    httpService.addItem(state.memo);
  }
  // 메모 삽입3
  // 안내 메시지 출력
  alert('저장했습니다.');
  // 메인 화면으로 이동
  router.push({ path: '/' });
};
onMounted(async () => {
  // 여기 아래 구문으로 분기를 줘서 // 추가하면 form화면 내용 없애고 있으면
  // 화면에 제목 내용을 출력하고 // 수정하는 화면도 하는거
  if (route.params.id) {
    // 값이 있다면 item 클릭, 없다면 [+추가하기] 버튼 클릭
    state.memo = await httpService.getItem(route.params.id);
    state.memo.id = parseInt(route.params.id);
  }
});
</script>
<template>
  <!-- 아래가 form의 전반적이 ㄴ화면 -->
  <h1>Form.vue</h1>
  <form class="detail" @submit.prevent="submit">
    <div class="mb-3" v-if="state.memo.createdAt">
      등록일시 : {{ state.memo.createdAt }}
    </div>
    <div class="mb-3">
      <label for="title" class="form-label">제목</label>
      <input
        type="text"
        id="title"
        class="form-control p-3"
        v-model="state.memo.title"
      />
    </div>
    <div class="mb-3">
      <label for="title" class="form-label">내용</label>
      <textarea
        id="content"
        class="form-control p-3"
        v-model="state.memo.content"
      ></textarea>
    </div>
    <button class="btn btn-primary w-100 py-3">저장</button>
  </form>
</template>
<style scoped></style>
