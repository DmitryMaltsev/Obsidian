 [SerializeField] Transform _worldSpacePosition;
    [SerializeField] Transform _localSpacePosition;
    [SerializeField] Transform _point;
    [SerializeField] Vector2 _pointPos;


    private void OnDrawGizmos()
    {
        DrawBasisVectors(_worldSpacePosition.position, _worldSpacePosition.right, _worldSpacePosition.up);
        DrawBasisVectors(_localSpacePosition.position, _localSpacePosition.right, _localSpacePosition.up);
        Vector2 offset = _localSpacePosition.position - _worldSpacePosition.position;
        Vector2 localToWorld = _localSpacePosition.right * _pointPos.x + _localSpacePosition.up * _pointPos.y;
        _point.position = localToWorld + offset;
    }

    private void DrawBasisVectors(Vector2 point, Vector2 right, Vector2 up)
    {
        Gizmos.color = Color.red;
        Gizmos.DrawRay(point, right);
        Gizmos.color = Color.green;
        Gizmos.DrawRay(point, up);
    }

    private Vector2 FindWorldPosition()
    {

        return new Vector2(0,0);
    }