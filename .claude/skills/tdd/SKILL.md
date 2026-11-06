--- 
name: tdd 
description: Enforce a disciplined Red-Green-Refactor loop with DAMP test readability, boundary-only mocking, and anti-rationalization guardrails.
--- 

# /tdd (Lean Hybrid Edition) 

## Objective 
Implement features or fix bugs strictly via Red-Green-Refactor in thin vertical slices while preventing agent shortcuts, ensuring DAMP readability, and isolating external boundaries correctly.

## Core Rules 
1. **Red-Green-Refactor Loop**: 
  - **RED**: Write a minimal failing test first. Run it to confirm it FAILS for the expected reason.
  - **GREEN**: Write the minimal production code to pass the test. Run it to confirm it PASSES.
  - **REFACTOR**: Clean up production &amp; test code without altering behavior. Re-verify tests remain green.   
2. **DAMP over DRY in Tests**: Prioritize readability and self-contained setup (*Descriptive And Meaningful Phrases*) over strict DRY abstractions in test suites. Tests must serve as unambiguous documentation.
3. **Mock Boundaries Only**: Only mock external dependencies (HTTP APIs, Database I/O, Timers). Never mock internal domain logic or the subject under test. 
4. **One Vertical Slice at a Time**: Never write tests or logic for multiple unrelated features in a single iteration.

## Anti-Rationalization Guardrails
Do NOT skip steps. Override common AI excuses with these mandatory rebuttals: 
| Agent Excuse | Mandatory Rebuttal |
| :--- | :--- | 
| *"This change is too trivial to write a test for."* | **No test, no code.** Small changes break edge cases; write a failing test first. |
| *"I verified the logic manually in my context."* | **Manual checks are not proof.** Execute an automated test and output execution logs. | 
| *"I'll write implementation first, then add tests."* | **Strict TDD required.** Code-first risks confirmation bias and false-green tests. | 
| *"I should create a helper/abstraction for setup."* | **DAMP over DRY.** Keep setup explicit in test blocks so failure context is instantly clear. | 
| *"I will mock the internal module to speed up test."* | **Over-mocking prohibited.** Mock external boundaries only. Domain logic must run for real. |

## Mocking Principles 
- **What to Mock**: External HTTP endpoints, Database I/O, File System, Network Sockets, System Clocks. 
- **What NOT to Mock**: Domain entities, pure functions, state transitions, or internal implementation details. 
- **Error Injection**: Include tests for boundary failures (e.g., Network Timeout, DB Disconnect, 500 Server Error). 

## Test Examples (DAMP & AAA Pattern) 

### ❌ Anti-Pattern (Over-DRY &amp; Over-Mocked) 
```ts 
// BAD: Obscure setup hidden in global hooks, internal logic mocked beforeEach(() => setupGlobalMocks()); 
test('user test', () => { 
  jest.spyOn(UserService.prototype, 'calculateDiscount').mockReturnValue(10); 
// BAD: Mocked internal domain! 
  expect(processUser()).toBeDefined(); // BAD: Weak assertion });
```

### ✅ Recommended Pattern (DAMP + Boundary Mocking)
```ts
// GOOD: Explicit Arrange-Act-Assert, self-contained, boundary-only mock 
test('should apply 15% discount when checkout total exceeds \\$100', async () => 
{ 
  // Arrange: Boundary mock for external Payment Gateway only 
  const mockPaymentGateway = { process: jest.fn().mockResolvedValue({ status: 'SUCCESS' }) }; 
  const cart = new ShoppingCart(); 
  cart.addItem({ name: 'Domain Architecture Book', price: 120 }); 
  
  // Act 
  const orderResult = await checkoutService.processOrder(cart, mockPaymentGateway); 
  
  // Assert: Explicit DAMP assertions checking exact business outcomes 
  expect(orderResult.finalAmount).toBe(102); // 120 \* 0.85 
  expect(orderResult.appliedDiscountRate).toBe(0.15); 
  expect(mockPaymentGateway.process).toHaveBeenCalledWith(102); 
});
```

## Execution Workflow 
1. **Locate or Create Test**: Target a single seam or vertical behavior.
2. **Execute & Prove Red**: Run test runner; confirm the failure log matches missing logic. 
3. **Implement & Prove Green**: Write minimal fix; run test runner; confirm pass log.
4. **Refactor & Verify**: Polish interface/structure; verify all tests remain green.