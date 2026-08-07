<pre>
using Microsoft.AspNetCore.Mvc;
using TmsApi.Application.Interfaces;

namespace TmsApi.Api.Controllers.V1;

[ApiController]
[Route("api/v1/enrollments")]


public class EnrollmentsController(
    IEnrollmentService service) : ControllerBase
{
    [HttpGet]
    public async Task<IActionResult> GetAll(
        CancellationToken cancellationToken)
    {
        var enrollments = await service.GetAllAsync(cancellationToken);

        return Ok(enrollments);
    }
}



namespace TmsApi.Application.DTOs;

public record EnrollmentDto(
    int Id,
    string StudentName,
    string CourseTitle,
    string CourseCode);



add inside IEnrollemenService.cs
Task<IReadOnlyList<EnrollmentDto>> GetAllAsync(CancellationToken cancellationToken);

---
add-services

  public async Task<IReadOnlyList<EnrollmentDto>> GetAllAsync(
        CancellationToken cancellationToken)
    {
        return await context.Enrollments
            .AsNoTracking()
            .Include(e => e.Student)
            .Include(e => e.Course)
            .Select(e => new EnrollmentDto(
                e.Id,
                e.Student.Name,
                e.Course.Title,
                e.Course.Code))
            .ToListAsync(cancellationToken);
    }
  </pre>
