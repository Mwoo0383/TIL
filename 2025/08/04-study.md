# 🏠 Room91 프로젝트

## 1. 전역 userRole 상태로 관리자 권한 제어

## 2. JWT 저장 키를 'token'에서 'jwt'로 전역 통일

### App.jsx
```JavaScript
const [userRole, setUserRole] = useState(null);

    useEffect(() => {
        const token = localStorage.getItem("jwt");
        if (token) {
            const parsed = parseJwt(token);
            setUserRole(parsed?.role || null);
        } else {
            setUserRole(null);
        }
    }, []);
```