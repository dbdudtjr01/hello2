<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <title>낭만 여행지 추천</title>
  <style>
    body {
      background-color: #000;
      color: #fff;
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      padding: 0;
    }

    header {
      text-align: center;
      padding: 40px 20px 20px;
    }

    h1 {
      font-size: 2.8rem;
      margin-bottom: 10px;
    }

    p.subtitle {
      font-size: 1.2rem;
      color: #ccc;
    }

    .input-box {
      text-align: center;
      margin-top: 30px;
    }

    input {
      padding: 10px;
      border: none;
      border-radius: 5px;
      width: 220px;
      font-size: 1rem;
    }

    button {
      padding: 10px 15px;
      margin-left: 10px;
      background-color: #fff;
      color: #000;
      border: none;
      border-radius: 5px;
      font-weight: bold;
      cursor: pointer;
    }

    .result {
      margin: 30px auto 20px;
      font-size: 1.1rem;
      max-width: 700px;
      text-align: left;
    }

    .place {
      margin-bottom: 15px;
    }

    .place strong {
      font-size: 1.2rem;
      display: block;
      margin-bottom: 5px;
      color: #fff;
    }

    .place p {
      margin: 0;
      color: #ccc;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 15px;
      max-width: 900px;
      margin: 30px auto 50px;
      padding: 0 20px;
    }

    .card {
      background-color: #1a1f3c; /* 짙은 남색 계열 */
      aspect-ratio: 1 / 1;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 1.3rem;
      border-radius: 10px;
      color: #fff;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    .card:hover {
      background-color: #2a2f4c;
    }
  </style>
</head>
<body>

  <header>
    <h1>낭만에 살고 낭만에 죽는 여행지</h1>
    <p class="subtitle">낭만이 넘치는 여행지와 설명을 알려드릴게요!!</p>
  </header>

  <div class="input-box">
    <input type="text" id="regionInput" placeholder="예: 강원도" />
    <button onclick="recommend()">추천 받기</button>
  </div>

  <div class="result" id="resultBox"></div>

  <div class="grid">
    <div class="card">서울</div>
    <div class="card">경기도</div>
    <div class="card">강원도</div>
    <div class="card">충청도</div>
    <div class="card">전라도</div>
    <div class="card">경상도</div>
    <div class="card">제주도</div>
    <div class="card">인천</div>
    <div class="card">세종시</div>
  </div>

  <script>
    const recommendations = {
      '서울': [
        { name: '북촌한옥마을', desc: '전통 한옥이 늘어선 골목길에서 한국의 옛 정취를 느낄 수 있어요.' },
        { name: '남산타워', desc: '서울의 전경이 한눈에 내려다보이는 낭만적인 전망 명소입니다.' },
        { name: '한강 반포공원', desc: '야경과 함께 물분수쇼를 즐기며 산책하기 좋은 장소입니다.' }
      ],
      '경기도': [
        { name: '아침고요수목원', desc: '사계절 꽃과 정원이 어우러진 자연 속 힐링 여행지입니다.' },
        { name: '헤이리 예술마을', desc: '예술과 감성이 흐르는 문화 복합 공간이에요.' },
        { name: '수원화성', desc: '세계문화유산으로 지정된 역사적인 성곽 도시입니다.' }
      ],
      '강원도': [
        { name: '경포대', desc: '바다와 호수, 그리고 노을이 어우러진 대표적 낭만 명소입니다.' },
        { name: '양떼목장', desc: '평창의 자연 속에서 귀여운 양들과 힐링 산책을 즐길 수 있어요.' },
        { name: '속초 해수욕장', desc: '푸른 동해와 함께하는 감성 가득한 바닷가입니다.' }
      ],
      '충청도': [
        { name: '공산성', desc: '백제의 역사가 살아 숨 쉬는 공주의 성곽입니다.' },
        { name: '도담삼봉', desc: '단양의 대표 경관으로 세 개의 봉우리가 강 위에 떠 있는 모습이 인상적이에요.' },
        { name: '상당산성', desc: '조용한 산책과 단풍 구경이 가능한 청주의 명소입니다.' }
      ],
      '전라도': [
        { name: '여수 밤바다', desc: '낭만 가득한 노래 속 명소, 야경이 아름다워요.' },
        { name: '전주 한옥마을', desc: '전통과 현대가 조화를 이루는 한국적인 마을입니다.' },
        { name: '담양 메타세쿼이아길', desc: '푸른 나무터널 아래에서 감성 사진 찍기 좋은 길입니다.' }
      ],
      '경상도': [
        { name: '경주 대릉원', desc: '천년의 역사와 고분이 어우러진 고즈넉한 공원입니다.' },
        { name: '부산 해운대', desc: '바다, 먹거리, 야경이 모두 있는 대표 해변 관광지입니다.' },
        { name: '통영 동피랑 마을', desc: '형형색색 벽화가 가득한 예쁜 언덕 마을입니다.' }
      ],
      '제주도': [
        { name: '성산 일출봉', desc: '해돋이 명소로 바다와 어우러진 장관을 볼 수 있어요.' },
        { name: '우도', desc: '제주 속의 작은 섬, 한적한 해변과 땅콩 아이스크림으로 유명해요.' },
        { name: '협재해수욕장', desc: '에메랄드 빛 바다와 부드러운 백사장이 인상적이에요.' }
      ],
      '인천': [
        { name: '차이나타운', desc: '한국 속 중국의 문화를 경험할 수 있는 거리입니다.' },
        { name: '송도 센트럴파크', desc: '도심 속 자연과 고급스러운 야경이 함께하는 공원입니다.' },
        { name: '월미도', desc: '놀이기구, 바다, 먹거리까지 즐길 수 있는 해변 놀이터입니다.' }
      ],
      '세종시': [
        { name: '세종호수공원', desc: '넓은 호수와 산책길, 야경이 아름다운 도심 공원입니다.' },
        { name: '국립세종도서관', desc: '독특한 건축미와 조용한 분위기의 문화 공간입니다.' },
        { name: '금강자연휴양림', desc: '자연과 함께 쉬어갈 수 있는 숲 속 힐링 공간입니다.' }
      ]
    };

    function recommend() {
      const input = document.getElementById('regionInput').value.trim();
      const resultBox = document.getElementById('resultBox');
      resultBox.innerHTML = '';

      if (recommendations[input]) {
        const places = recommendations[input];
        let html = `<h3> <strong>${input}</strong>의 추천 여행지:</h3>`;
        places.forEach(place => {
          html += `
            <div class="place">
              <strong>${place.name}</strong>
              <p>${place.desc}</p>
            </div>`;
        });
        resultBox.innerHTML = html;
      } else {
        resultBox.innerHTML = `❗ "<strong>${input}</strong>" 지역 정보가 없습니다. 정확히 입력해 주세요.`;
      }
    }
  </script>

</body>
</html>
