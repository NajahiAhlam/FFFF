this.getContratDetails().subscribe(() => {
    // Ensure contrat is populated before calling getUserName
    this.getUserName(this.contrat.userId);
  });

  <!-- contrat-details.component.html -->
<div *ngIf="userName$ | async as userName">
  <p>User Name: {{ userName }}</p>
</div>
