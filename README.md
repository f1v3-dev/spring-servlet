# spring-mvc

>만든 프레임워크 vs 스프링 MVC 비교
>- FrontController -> DispatcherServlet
>- handlerMappingMap -> HandlerMapping
>- MyHandlerAdapter -> HandlerAdapter
>- ModelView -> ModelAndView
>- viewResolver -> ViewResolver
>- MyView -> View 


### DispatcherServlet의 핵심 [dodispatch()]

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
  HttpServletRequest processedRequest = request;
  HandlerExecutionChain mappedHandler = null;
  ModelAndView mv = null;

  // 1. 핸들러 조회
  mappedHandler = getHandler(processedRequest);
  if (mappedHandler == null) {
    noHandlerFound(processedRequest, response);
    return;
  }
  
  // 2. 핸들러 어댑터 조회 - 핸들러를 처리할 수 있는 어댑터
  HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
  
  // 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환
  mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

  processDispatchResult(processedRequest, response, mappedHandler, mv, ispatchException);
}

private void processDispatchResult(HttpServletRequest request, ttpServletResponse response, 
      HandlerExecutionChain mappedHandler, ModelAndView mv, Exception exception) throws Exception {

  // 뷰 렌더링 호출
  render(mv, request, response);
}

protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {
  
  View view;
  String viewName = mv.getViewName();

  // 6. 뷰 리졸버를 통해서 뷰 찾기, 7. View 반환
  view = resolveViewName(viewName, mv.getModelInternal(), locale, request);

  // 8. 뷰 렌더링
  view.render(mv.getModelInternal(), request, response);
}

```

![image](https://user-images.githubusercontent.com/84575041/233353770-27780225-c5ff-4356-94b2-abe127e5b9c5.png)
