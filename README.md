# 📔 프레임워크 학습

# 🔸 사용 프레임워크

## Front-End

- React
- Redux
  - 액션
    - 상태에 어떠한 변화가 필요하면 액션(action) 발생
    - 이를 액션 객체로 표현
      ```jsx
      {
        type: "ADD_COMMENT";
      }
      ```
    - 액션 객체에 type field는 반드시 필요, 이를 액션의 이름으로 생각하면 됨
  - 액션 생성 함수
    - 액션 객체를 만들어 주는 함수
      ```jsx
      function addTodo(data) {
        return {
          type: "ADD_COMMENT",
          data,
        };
      }
      ```
  - 리듀서
    - 변화를 일으키는 함수로, 액션이 발생하면 리듀서가 현재 상태와 전달받은 액션 객체를 파라미터로 받아옴
    - 현재 상태, 액션 객체 파라미터를 참고하여 새로운 상태를 반환
      ```jsx
      const initialState = {
      	cnt: 1
      };
      function reducer (state = initialState, action) {
      	swtich(action.type) {
      		case INCREASE:
      			cnt: state.cnt + 1
      		};
      		default:
      			return state;
      	}
      }
      ```
  - 스토어
    - 한 개의 프로젝트는 한 개의 스토어만 가질 수 있음
    - 스토어 안에는 현재 state와 리듀서 등이 들어있음
  - 디스패치
    - 스토어의 내장함수로, 액션을 발생시킴
    - dispatch(action)과 같은 형태로 호출
- Axios
  ```jsx
  import axios from "axios";

  const BASE_URL = "http://localhost:3000";

  // Function to load comments for a specific post
  export const loadComments = async (postId) => {
    try {
      const response = await axios.get(
        `${BASE_URL}/api/posts/${postId}/comments`
      );
      console.log(response);
      return response.data;
    } catch (error) {
      throw error.response ? error.response.data : error.message;
    }
  };
  ```
  - Promises 기반 API
    - **`async/await`**를 사용하여 비동기 코드를 작성할 수 있음
  - 단순하고 일관된 API
    - XMLHttpRequest와 같은 복잡한 부분을 추상화하고 코드를 깔끔하게 유지할 수 있음
      ```jsx
      const BASE_URL = "http://localhost:3000";

      // Function to load comments for a specific post
      export const loadComments = (postId, callback) => {
        const xhr = new XMLHttpRequest();

        // Define the callback function to handle the response
        xhr.onreadystatechange = function () {
          if (xhr.readyState === XMLHttpRequest.DONE) {
            if (xhr.status === 200) {
              // Successful response
              const responseData = JSON.parse(xhr.responseText);
              console.log(responseData);
              callback(null, responseData);
            } else {
              // Error response
              const errorMessage = xhr.status + " " + xhr.statusText;
              callback(errorMessage, null);
            }
          }
        };

        // Open a GET request to the specified URL
        xhr.open("GET", `${BASE_URL}/api/posts/${postId}/comments`, true);

        // Send the request
        xhr.send();
      };

      // Example usage
      loadComments(1, (error, data) => {
        if (error) {
          console.error("Error loading comments:", error);
        } else {
          console.log("Comments loaded successfully:", data);
        }
      });
      ```
- Immer
  - 리덕스에서는 불변성을 중시하여 state를 직접 변경하는 것을 권장하지 않음
  - 새로운 상태 객체를 반환하는 형식으로 상태를 업데이트
  - immer의 produce 함수가 이 작업을 간편하게 해줌
  ```jsx
  const auth = handleActions({
    [CHANGE_FIELD]: (state, { payload: { form, key, value } }) =>
      produce(state, (draft) => {
        draft[form][key] = value;
      }),
  });
  ```
- Quill
- sanitize-html
  - HTML에서 악의적인 스크립트나 기타 보안 문제를 감지하기 위한 도구
  - 이번 프로젝트에서는 Post의 body를 해당 라이브러리를 통해 보호

## Back-End

- Express.js
- Mongoose
- JSON Web Token
- bcrypt
  - 비밀번호 해싱에 사용되는 강력하고 안전한 방법을 제공하는 라이브러리 중 하나
    ```jsx
    UserSchema.methods.setPassword = async function (password) {
      const hash = await bcrypt.hash(password, saltRounds);
      this.hashedPassword = hash;
    };
    ```
- Joi and @hapi/joi
  - 설정한 객체의 스키마에 부합하는 데이터 구조인지 유효성을 검사
    ```jsx
    /* POST /api/auth/register */
    export const register = async (req, res) => {
      // validate request body
      const schema = Joi.object().keys({
        username: Joi.string().alphanum().min(3).max(20).required(),
        password: Joi.string().required(),
      });
    ```
