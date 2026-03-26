[Fact]
```
public async void GetForUser()
{
    // Arrange - set up fake data and services to simulate the database
    var goals = collections.GetGoals();
    var users = collections.GetUsers();
    IGoalsService goalsService = new FakeGoalsService(goals, goals[0]);
    IUsersService usersService = new FakeUsersService(users, users[0]);
    GoalController controller = new(goalsService, usersService);

    // Act - call the GetForUser route with the first goal's UserId
    var httpContext = new Microsoft.AspNetCore.Http.DefaultHttpContext();
    controller.ControllerContext.HttpContext = httpContext;
    var result = await controller.GetForUser(goals[0].UserId!);

    // Assert - make sure the result is not null
    Assert.NotNull(result);

    // Loop through each goal and check:
    // 1. It is a valid Goal object
    // 2. It belongs to the expected user
    var index = 0;
    foreach (Goal goal in result!)
    {
        Assert.IsAssignableFrom<Goal>(goal);
        Assert.Equal(goals[0].UserId, goal.UserId);
        index++;
    }
}
```