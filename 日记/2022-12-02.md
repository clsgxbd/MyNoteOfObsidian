
#thymeleaf

- [x] 完成sku制作保存
- [x] 完成sku列表和上下架功能
- [x] ## thymeleaf 语法练习3

thymeleaf 前端
```
- [ ] th:text="${name}"// 可以将响应的内容以文本方
- [ ] th:text="${session.username}"
- [ ] th:each"item,stat : ${list}"
- [ ] th:if="${age>=18}" 或者 th:if="${age}>=18"
- [ ] th:unless="${age>=18}"  或者th:unless="${age}>=18"
- [ ] th:include="foot"
- [ ] th:utext="${text}" // 可以将响应的样式进行解析后显示
- [ ] th:href="@{http://localhost:8081/list.html?category3Id={aaa}(aaa=${category3Id})}" // 跳转界面

thymeleaf 后端
- [ ] Model model 
- [ ] model.addAttribute("name","damu")
- [ ] model.addAttribute("name",list)
- [ ] HttpSession session
- [ ] session.setAttribute("username","damu")
- [ ] Map<String, Object> map
- [ ] map.put("name", "damu")
- [ ] HttpServletRequest request
- [ ] request.setAttribute("name","damu")
```





